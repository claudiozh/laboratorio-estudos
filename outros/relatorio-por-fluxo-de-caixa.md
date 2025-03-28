---
icon: book-open
---

# Relatório por fluxo de caixa

````
```typescript
import { Injectable } from '@nestjs/common';
import { Prisma } from '@prisma/client';

import { dateManager } from '@/common/utils/date-manager';
import { PrismaService } from '@/modules/database/prisma/prisma.service';
import {
  calcAHPercent,
  createInitialValuesCashFlowReport,
  getFormattedPeriodByVision,
} from '@/modules/database/prisma/utils/reports';
import { GetCashFlowReportParamsDTO } from '@/modules/reports/dtos/get-cash-flow-report-params.dto';
import { GetDreReportParamsDTO } from '@/modules/reports/dtos/get-dre-report-params.dto';
import { TypeCashFlowEnum } from '@/modules/reports/enums/type-cash-flow.enum';
import { TypeDreCategoryEnum } from '@/modules/reports/enums/type-dre-category.enum';
import { VisionReportEnum } from '@/modules/reports/enums/vision-report.enum';
import { groupedPeriod } from '@/modules/reports/use-cases/get-dre-report.usecase';

export interface ISumTransactionsPerDreCategoryOutput {
  dre_category_id: string;
  dre_category_type: string;
  date_group: string;
  dre_category_alias: string | null;
  dre_parent_id: string;
  dre_category_name: string;
  dre_order: bigint;
  total: bigint;
}

export interface IGetSumTransactionPerDreCategoryOutput {
  dreCategoryAlias: string | null;
  dreCategoryId: string;
  dreCategoryName: string;
  dreCategoryType: string;
  dreParentId: string;
  dateGroup: string;
  dreOrder: number;
  total: number;
}

export interface ISumTransactionsByCategoryDatabase {
  category_id: string;
  category_type: string;
  category_name: string;
  category_parent_id: string;
  date_group: string;
  invoice_status: string;
  amount_in_cents: bigint;
  paid_amount_in_cents: bigint;
}

export interface ISumBankAccountBalanceDatabase {
  date_group: Date;
  total_initial_balance: bigint;
}

interface ICategoryReport {
  id: string;
  name: string;
  type: 'CREDIT' | 'DEBIT';
  amountInCents: number;
  ahAmountInCents: number;
  paidAmountInCents: number;
  ahPaidAmountInCents: number;
  parentId: string;
  date: string;
}

export interface IReportCashFlow {
  [key: string]: {
    isToShow: boolean;
    isActualPeriod: boolean;
    periodGraphic: string;
    periodTable: string;

    previousBalanceInCents: number;
    previousBalancePaidInCents: number;
    ahPreviousBalanceInCents: number;
    ahPreviousBalancePaidInCents: number;

    initialBalanceInCents: number; // Saldo Inicial
    initialBalancePaidInCents: number;
    ahInitialBalanceInCents: number;
    ahInitialBalancePaidInCents: number;

    totalGeralInCents: number;
    totalGeralPaidInCents: number;
    ahTotalGeralPaidInCents: number;
    ahTotalGeralInCents: number;

    totalBalancesInCents: number;
    totalBalancesPaidInCents: number;
    ahTotalBalancesInCents: number;
    ahTotalBalancesPaidInCents: number;

    totalRecipesInCents: number;
    totalRecipesPaidInCents: number;
    ahTotalRecipesInCents: number;
    ahTotalRecipesPaidInCents: number;

    totalExpensesInCents: number;
    totalExpensesPaidInCents: number;
    ahTotalExpensesInCents: number;
    ahTotalExpensesPaidInCents: number;

    recipes: ICategoryReport[];
    expenses: ICategoryReport[];
  };
}

export interface ISumTransactionsByCategoryOutput {
  categoryId: string;
  categoryType: string;
  categoryName: string;
  categoryParentId: string;
  invoiceStatus: string;
  dateGroup: string;
  total: number;
}

const period: groupedPeriod = {
  [VisionReportEnum.MONTHLY]: 'month',
  [VisionReportEnum.YEARLY]: 'year',
  [VisionReportEnum.QUARTER]: 'quarter',
  [VisionReportEnum.SEMESTER]: 'semester',
};

@Injectable()
export class ReportsRepository {
  constructor(private readonly prismaService: PrismaService) {}

  async getSumTransactionsPerDreCategory({
    endDate,
    startDate,
    vision,
    type,
  }: GetDreReportParamsDTO): Promise<IGetSumTransactionPerDreCategoryOutput[]> {
    const isCashType = type === TypeDreCategoryEnum.CASH_FLOW;

    const sumTransactionsPerCategory = await this.prismaService.$queryRaw<ISumTransactionsPerDreCategoryOutput[]>`
    -- Busca todas as categorias que possuem transações
    SELECT 
        d.id AS dre_category_id,
        d.type AS dre_category_type,
        d.alias AS dre_category_alias,
        d.name AS dre_category_name,
        d.order AS dre_order,
        d.dre_parent_id AS dre_parent_id,
        CASE 
    WHEN ${period[vision]} = 'semester' THEN 
        TO_CHAR(t.competition_date, 'YYYY"-"') || LPAD(CAST(FLOOR((EXTRACT(MONTH FROM t.competition_date) - 1) / 6 + 1) AS TEXT), 2, '0')
    ELSE 
        TO_CHAR(DATE_TRUNC(${period[vision]}, t.competition_date), 'YYYY-MM-DD')
    END AS date_group,
        SUM(t.amount_in_cents) AS total
    FROM dre_categories d
    JOIN categories c ON d.id = c.dre_category_id
    JOIN transactions t ON c.id = t.category_id
    JOIN invoices i ON t.invoice_id = i.id
    WHERE t.competition_date BETWEEN ${dateManager(startDate).toDate()} AND ${dateManager(endDate).toDate()}
    AND (
        NOT ${isCashType} OR i.status = 'PAID'
    )
    GROUP BY date_group, d.id
    
    UNION ALL
  
    -- Busca todas as categorias da DRE que não possuem transações e força o total para 0
    SELECT 
        d.id AS dre_category_id,
        d.type AS dre_category_type,
        d.alias AS dre_category_alias,
        d.name AS dre_category_name,
        d.order AS dre_order,
        d.dre_parent_id AS dre_parent_id,
        NULL AS date_group, 
        0 AS total
    FROM dre_categories d
    WHERE d.id NOT IN (
        SELECT DISTINCT d.id
        FROM dre_categories d
        JOIN categories c ON d.id = c.dre_category_id
        JOIN transactions t ON c.id = t.category_id
        JOIN invoices i ON t.invoice_id = i.id
        WHERE t.competition_date BETWEEN ${dateManager(startDate).toDate()} AND ${dateManager(endDate).toDate()}
        AND (
        NOT ${isCashType} OR i.status = 'PAID'
        ) 
    )
    
    ORDER BY dre_order ASC NULLS LAST;
  `;

    return sumTransactionsPerCategory.map((value) => ({
      dreCategoryAlias: value.dre_category_alias,
      dreCategoryId: value.dre_category_id,
      dreCategoryName: value.dre_category_name,
      dreCategoryType: value.dre_category_type,
      dateGroup: value.date_group,
      total: Number(value.total),
      dreParentId: value.dre_parent_id,
      dreOrder: value.dre_order ? Number(value.dre_order) : null,
    }));
  }

  async getReportCashFlow(params: GetCashFlowReportParamsDTO): Promise<IReportCashFlow> {
    const { vision, type } = params;
    const startDate = dateManager(params.startDate).toDate();
    const endDate = dateManager(params.endDate).toDate();
    const invoiceStatus = type === TypeCashFlowEnum.PAID ? ['PAID'] : ['PAID', 'AWAITING_PAYMENT'];

    const sumTransactionsPerCategory = await this.prismaService.$queryRaw<ISumTransactionsByCategoryDatabase[]>`
        SELECT
            c.id AS category_id,
            c.type::TEXT AS category_type,
            c.name AS category_name,
            c.parent_id AS category_parent_id,
            i.status AS invoice_status,
            DATE_TRUNC('month', t.competition_date) AS date_group,
            SUM(t.amount_in_cents) AS amount_in_cents,
            SUM(CASE WHEN i.status = 'PAID' THEN t.amount_in_cents ELSE 0 END) AS paid_amount_in_cents
        FROM categories c
        LEFT JOIN categories child ON c.id = child.parent_id
        JOIN transactions t ON t.category_id = c.id OR t.category_id = child.id
        JOIN invoices i ON t.invoice_id = i.id
        WHERE c.type IN ('CREDIT', 'DEBIT')
            AND t.competition_date <= ${endDate}
            AND i.status::text IN (${Prisma.join(invoiceStatus)})
        GROUP BY date_group, c.id, c.type, c.name, c.parent_id, i.status

        UNION ALL

        SELECT
            b.id AS category_id,
            CAST('INITIAL_BALANCE' AS TEXT) AS category_type,
            'Saldo Inicial' AS category_name,
            NULL AS category_parent_id,
            'PAID' AS invoice_status,
            DATE_TRUNC('month', b.created_at) AS date_group,
            (b.initial_balance_in_cents) AS amount_in_cents,
            (b.initial_balance_in_cents) AS paid_amount_in_cents
        FROM bank_accounts b
        WHERE b.created_at <= ${endDate}

        ORDER BY date_group ASC,
                category_parent_id NULLS FIRST,
                category_id;
    `;

    const firstTransactionPerCategory = sumTransactionsPerCategory[0];
    const firstItemDate = firstTransactionPerCategory
      ? dateManager(dateManager(firstTransactionPerCategory.date_group).tz('UTC').format('YYYY-MM-DD')).toDate()
      : null;
    const minDate = firstItemDate && firstItemDate < startDate ? firstItemDate : startDate;

    const reportCashFlow = createInitialValuesCashFlowReport(vision, {
      startDateToGenerate: minDate,
      startDateToShow: startDate,
      endDate,
    });

    sumTransactionsPerCategory.forEach((category) => {
      const dateGroup = dateManager(category.date_group).tz('UTC').format('YYYY-MM-DD');
      const dateGroupWithoutUTC = dateManager(dateGroup).toDate();
      const { periodKey, periodGraphic, periodTable } = getFormattedPeriodByVision(dateGroupWithoutUTC, vision);

      const reportItem = reportCashFlow[periodKey];
      const expenses = [...reportItem.expenses];
      const recipes = [...reportItem.recipes];

      let totalRecipesInCents = reportItem.totalRecipesInCents || 0;
      let totalRecipesPaidInCents = reportItem.totalRecipesPaidInCents || 0;
      let totalExpensesInCents = reportItem.totalExpensesInCents || 0;
      let totalExpensesPaidInCents = reportItem.totalExpensesPaidInCents || 0;
      let initialBalanceInCents = reportItem.initialBalanceInCents || 0;
      let initialBalancePaidInCents = reportItem.initialBalancePaidInCents || 0;

      const processCategoryByType = {
        INITIAL_BALANCE: () => {
          initialBalanceInCents += Number(category.amount_in_cents);
          initialBalancePaidInCents += Number(category.paid_amount_in_cents);
        },
        DEBIT: () => {
          const expenseAlreadyExists = expenses.find((expense) => expense.id === category.category_id);
          const isCategoryFather = category.category_parent_id === null;

          if (isCategoryFather) {
            totalExpensesInCents += Number(category.amount_in_cents);
            totalExpensesPaidInCents += Number(category.paid_amount_in_cents);
          }

          if (expenseAlreadyExists) {
            expenseAlreadyExists.amountInCents += Number(category.amount_in_cents);
            expenseAlreadyExists.paidAmountInCents += Number(category.paid_amount_in_cents);
            return;
          }

          expenses.push({
            id: category.category_id,
            name: category.category_name,
            type: 'CREDIT',
            parentId: category.category_parent_id,
            date: category.date_group,
            amountInCents: Number(category.amount_in_cents),
            paidAmountInCents: Number(category.paid_amount_in_cents),
            ahPaidAmountInCents: 0,
            ahAmountInCents: 0,
          });
        },
        CREDIT: () => {
          const recipeAlreadyExists = recipes.find((recipe) => recipe.id === category.category_id);
          const isCategoryFather = category.category_parent_id === null;

          if (isCategoryFather) {
            totalRecipesInCents += Number(category.amount_in_cents);
            totalRecipesPaidInCents += Number(category.paid_amount_in_cents);
          }

          if (recipeAlreadyExists) {
            recipeAlreadyExists.amountInCents += Number(category.amount_in_cents);
            recipeAlreadyExists.paidAmountInCents += Number(category.paid_amount_in_cents);
            return;
          }

          recipes.push({
            id: category.category_id,
            name: category.category_name,
            type: 'DEBIT',
            parentId: category.category_parent_id,
            date: category.date_group,
            amountInCents: Number(category.amount_in_cents),
            paidAmountInCents: Number(category.paid_amount_in_cents),
            ahAmountInCents: 0,
            ahPaidAmountInCents: 0,
          });
        },
      };

      processCategoryByType[category.category_type]();

      reportCashFlow[periodKey] = {
        ...reportItem,
        periodGraphic,
        periodTable,
        initialBalanceInCents,
        initialBalancePaidInCents,
        totalRecipesInCents,
        totalRecipesPaidInCents,
        totalExpensesInCents,
        totalExpensesPaidInCents,
        expenses,
        recipes,
      };
    });

    const keys = Object.keys(reportCashFlow);

    keys.forEach((key, index) => {
      const reportItem = reportCashFlow[key];
      const previousReportItem = reportCashFlow[keys[index - 1]];

      const previousTotalBalancesInCents = previousReportItem?.totalBalancesInCents || 0;
      const previousPreviousBalanceInCents = previousReportItem?.previousBalanceInCents || 0;
      const previousInitialBalanceInCents = previousReportItem?.initialBalanceInCents || 0;
      const previousTotalGeralInCents = previousReportItem?.totalGeralInCents || 0;
      const previousTotalGeralPaidInCents = previousReportItem?.totalGeralPaidInCents || 0;
      const previousTotalRecipesInCents = previousReportItem?.totalRecipesInCents || 0;
      const previousTotalRecipesPaidInCents = previousReportItem?.totalRecipesPaidInCents || 0;
      const previousTotalExpensesInCents = previousReportItem?.totalExpensesInCents || 0;
      const previousTotalExpensesPaidInCents = previousReportItem?.totalExpensesPaidInCents || 0;

      // Calculo de total de saldos = Total geral do período anterior + Saldo inicial
      reportItem.totalBalancesInCents = previousTotalGeralInCents + reportItem.initialBalanceInCents;
      reportItem.totalBalancesPaidInCents = previousTotalGeralPaidInCents + reportItem.initialBalancePaidInCents;

      const ahTotalBalancesInCents = calcAHPercent(reportItem.totalBalancesInCents, previousTotalBalancesInCents);
      reportItem.ahTotalBalancesInCents = ahTotalBalancesInCents;
      reportItem.ahTotalBalancesPaidInCents = ahTotalBalancesInCents;

      // Calculo de Saldo anterior = Total geral do período anterior
      reportItem.previousBalanceInCents = previousTotalGeralInCents;
      reportItem.previousBalancePaidInCents = previousTotalGeralPaidInCents;

      const ahPreviousBalanceInCents = calcAHPercent(previousPreviousBalanceInCents, previousPreviousBalanceInCents);
      reportItem.ahPreviousBalanceInCents = ahPreviousBalanceInCents;
      reportItem.ahPreviousBalancePaidInCents = ahPreviousBalanceInCents;

      // Calculo de total geral
      reportItem.totalGeralInCents =
        reportItem.totalBalancesInCents + reportItem.totalRecipesInCents + reportItem.totalExpensesInCents;

      reportItem.totalGeralPaidInCents =
        reportItem.totalBalancesPaidInCents + reportItem.totalRecipesPaidInCents + reportItem.totalExpensesPaidInCents;

      reportItem.ahTotalGeralInCents = calcAHPercent(reportItem.totalGeralInCents, previousTotalGeralInCents);
      reportItem.ahTotalGeralPaidInCents = calcAHPercent(
        reportItem.totalGeralPaidInCents,
        previousTotalGeralPaidInCents,
      );

      // Calculo para o AH do initial balance
      const ahInitialBalanceInCents = calcAHPercent(reportItem.initialBalanceInCents, previousInitialBalanceInCents);
      reportItem.ahInitialBalanceInCents = ahInitialBalanceInCents;
      reportItem.ahInitialBalancePaidInCents = ahInitialBalanceInCents;

      // Calculo para o AH do total de receitas
      reportItem.ahTotalRecipesInCents = calcAHPercent(reportItem.totalRecipesInCents, previousTotalRecipesInCents);
      reportItem.ahTotalRecipesPaidInCents = calcAHPercent(
        reportItem.totalRecipesPaidInCents,
        previousTotalRecipesPaidInCents,
      );

      // Calculo para o AH do total de despesas
      reportItem.ahTotalExpensesInCents = calcAHPercent(reportItem.totalExpensesInCents, previousTotalExpensesInCents);
      reportItem.ahTotalExpensesPaidInCents = calcAHPercent(
        reportItem.totalExpensesPaidInCents,
        previousTotalExpensesPaidInCents,
      );

      const { recipes, expenses } = reportItem;

      recipes.forEach((recipe) => {
        const recipePrevious = previousReportItem?.recipes?.find((r) => r.id === recipe.id);

        recipe.ahAmountInCents = calcAHPercent(recipe.amountInCents, recipePrevious?.amountInCents || 0);
        recipe.ahPaidAmountInCents = calcAHPercent(recipe.paidAmountInCents, recipePrevious?.paidAmountInCents || 0);
      });

      expenses.forEach((expense) => {
        const expensePrevious = previousReportItem?.expenses?.find((r) => r.id === expense.id);

        expense.ahAmountInCents = calcAHPercent(expense.amountInCents, expensePrevious?.amountInCents || 0);
        expense.ahPaidAmountInCents = calcAHPercent(expense.paidAmountInCents, expensePrevious?.paidAmountInCents || 0);
      });

      if (previousReportItem?.isToShow === false) {
        delete reportCashFlow[keys[index - 1]];
      }
    });

    return reportCashFlow;
  }
}

```
````

