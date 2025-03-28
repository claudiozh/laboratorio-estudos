---
icon: book-open
---

# Relatório por fluxo de caixa

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


@Injectable()
export class ReportsRepository {
  constructor(private readonly prismaService: PrismaService) {}

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

{% code title="utils.ts" %}
```typescript
import { IReportCashFlow } from '@/modules/database/prisma/repositories/reports.repository';
import { VisionReportEnum } from '@/modules/reports/enums/vision-report.enum';

interface ICreateInitialValuesCashFlow {
  startDateToGenerate: Date;
  startDateToShow: Date;
  endDate: Date;
}

const months = ['Jan', 'Fev', 'Mar', 'Abr', 'Mai', 'Jun', 'Jul', 'Ago', 'Set', 'Out', 'Nov', 'Dez'];
const quartesLabels = ['01 Jan à 31 Mar', '01 Abr à 30 Jun', '01 Jul à 30 Set', '01 Out à 31 Dez'];
const semestersLabels = ['01 Jan à 30 Jun', '01 Jul à 31 Dez'];

export const defaultValuesReportCashFlow: IReportCashFlow[string] = {
  isToShow: true,
  isActualPeriod: false,
  periodGraphic: '',
  periodTable: '',
  initialBalanceInCents: 0,
  ahInitialBalancePaidInCents: 0,
  ahPreviousBalancePaidInCents: 0,
  initialBalancePaidInCents: 0,
  previousBalancePaidInCents: 0,
  totalGeralInCents: 0,
  totalBalancesInCents: 0,
  previousBalanceInCents: 0,
  ahPreviousBalanceInCents: 0,
  ahTotalBalancesInCents: 0,
  ahTotalGeralInCents: 0,
  ahTotalGeralPaidInCents: 0,
  ahInitialBalanceInCents: 0,
  totalGeralPaidInCents: 0,
  ahTotalBalancesPaidInCents: 0,
  totalBalancesPaidInCents: 0,
  totalRecipesInCents: 0,
  totalRecipesPaidInCents: 0,
  totalExpensesInCents: 0,
  totalExpensesPaidInCents: 0,
  ahTotalRecipesInCents: 0,
  ahTotalRecipesPaidInCents: 0,
  ahTotalExpensesInCents: 0,
  ahTotalExpensesPaidInCents: 0,
  recipes: [],
  expenses: [],
};

export const getMonthlyPeriod = (date: Date) => {
  const year = date.getFullYear();
  const month = date.getMonth(); // Janeiro é 0, Fevereiro é 1, etc.
  const monthLabel = months[month];

  return {
    periodGraphic: `${monthLabel}/${year}`,
    periodTable: `${monthLabel} de ${year}`,
    periodKey: `monthly-${year}-${String(month + 1).padStart(2, '0')}`,
  };
};

export const getQuarterlyPeriod = (date: Date) => {
  const year = date.getFullYear();
  const month = date.getMonth();
  const quarter = Math.ceil((month + 1) / 3); // 1-3 para 1º tri, 4-6 para 2º tri, etc.

  return {
    periodGraphic: `${quarter}º tri/${String(year).slice(-2)}`,
    periodTable: `${quartesLabels[quarter - 1]} (${year})`,
    periodKey: `quarterly-${quarter}/${year}`,
  };
};

export const getSemesterPeriod = (date: Date) => {
  const year = date.getFullYear();
  const month = date.getMonth();
  const semester = Math.ceil((month + 1) / 6); // 1-6 para 1º sem, 7-12 para 2º sem, etc.

  return {
    periodGraphic: `${semester}º sem/${String(year).slice(-2)}`,
    periodTable: `${semestersLabels[semester - 1]} (${year})`,
    periodKey: `semester-${semester}/${year}`,
  };
};

export const getYearlyPeriod = (date: Date) => {
  const year = date.getFullYear();

  return {
    periodGraphic: year.toString(),
    periodTable: year.toString(),
    periodKey: `yearly-${year}`,
  };
};

export const getFormattedPeriodByVision = (date: Date, vision: VisionReportEnum) => {
  switch (vision) {
    case VisionReportEnum.MONTHLY:
      return getMonthlyPeriod(date);
    case VisionReportEnum.QUARTER:
      return getQuarterlyPeriod(date);
    case VisionReportEnum.SEMESTER:
      return getSemesterPeriod(date);
    case VisionReportEnum.YEARLY:
      return getYearlyPeriod(date);
    default:
      throw new Error('Visão inválida');
  }
};

export const createInitialValuesCashFlowByMonth = (params: ICreateInitialValuesCashFlow): IReportCashFlow => {
  const { startDateToGenerate, startDateToShow, endDate } = params;
  const initialValues: IReportCashFlow = {};

  const startYear = startDateToGenerate.getFullYear();
  const startMonth = startDateToGenerate.getMonth() + 1;

  const endYear = endDate.getFullYear();
  const endMonth = endDate.getMonth() + 1;

  const currentDate = new Date();
  const { periodKey: periodKeyCurrentDate } = getMonthlyPeriod(currentDate);

  for (let year = startYear; year <= endYear; year++) {
    const monthStart = year === startYear ? startMonth : 1;
    const monthEnd = year === endYear ? endMonth : 12;

    for (let month = monthStart; month <= monthEnd; month++) {
      const date = new Date(year, month - 1, 1);
      const { periodGraphic, periodTable, periodKey } = getMonthlyPeriod(date);

      initialValues[periodKey] = {
        ...defaultValuesReportCashFlow,
        isToShow: date >= startDateToShow && date <= endDate,
        isActualPeriod: periodKey === periodKeyCurrentDate,
        periodGraphic: `${periodGraphic}`,
        periodTable: `${periodTable}`,
      };
    }
  }

  return initialValues;
};

export const createInitialValuesCashFlowByQuarter = (params: ICreateInitialValuesCashFlow): IReportCashFlow => {
  const { startDateToGenerate, startDateToShow, endDate } = params;
  const initialValues: IReportCashFlow = {};

  const startYear = startDateToGenerate.getFullYear();
  const startMonth = Math.floor(startDateToGenerate.getMonth() / 3) + 1;

  const endYear = endDate.getFullYear();
  const endMonth = Math.floor(endDate.getMonth() / 3) + 1;

  const currentDate = new Date();
  const { periodKey: periodKeyCurrentDate } = getQuarterlyPeriod(currentDate);

  for (let year = startYear; year <= endYear; year++) {
    const trimesterStart = year === startYear ? startMonth : 1;
    const trimesterEnd = year === endYear ? endMonth : 4;

    for (let trimester = trimesterStart; trimester <= trimesterEnd; trimester++) {
      const date = new Date(year, (trimester - 1) * 3, 1);
      const endDateTrimester = new Date(year, (trimester - 1) * 3 + 2, 31);
      const { periodGraphic, periodTable, periodKey } = getQuarterlyPeriod(date);

      initialValues[periodKey] = {
        ...defaultValuesReportCashFlow,
        isToShow: endDateTrimester >= startDateToShow && endDateTrimester <= endDate,
        isActualPeriod: periodKey === periodKeyCurrentDate,
        periodGraphic: `${periodGraphic}`,
        periodTable: `${periodTable}`,
      };
    }
  }

  return initialValues;
};

export const createInitialValuesCashFlowBySemester = (params: ICreateInitialValuesCashFlow): IReportCashFlow => {
  const { startDateToGenerate, startDateToShow, endDate } = params;
  const initialValues: IReportCashFlow = {};

  const startYear = startDateToGenerate.getFullYear();
  const startMonth = Math.floor(startDateToGenerate.getMonth() / 6) + 1;

  const endYear = endDate.getFullYear();
  const endMonth = Math.floor(endDate.getMonth() / 6) + 1;

  const currentDate = new Date();
  const { periodKey: periodKeyCurrentDate } = getSemesterPeriod(currentDate);

  for (let year = startYear; year <= endYear; year++) {
    const semesterStart = year === startYear ? startMonth : 1;
    const semesterEnd = year === endYear ? endMonth : 2;

    for (let semester = semesterStart; semester <= semesterEnd; semester++) {
      const date = new Date(year, (semester - 1) * 6, 1);
      const endDateSemester = new Date(year, (semester - 1) * 6 + 5, 31);
      const { periodGraphic, periodTable, periodKey } = getSemesterPeriod(date);

      initialValues[periodKey] = {
        ...defaultValuesReportCashFlow,
        isToShow: endDateSemester >= startDateToShow && endDateSemester <= endDate,
        isActualPeriod: periodKey === periodKeyCurrentDate,
        periodGraphic: `${periodGraphic}`,
        periodTable: `${periodTable}`,
      };
    }
  }

  return initialValues;
};

export const createInitialValuesCashFlowByYear = (params: ICreateInitialValuesCashFlow): IReportCashFlow => {
  const { startDateToGenerate, startDateToShow, endDate } = params;
  const initialValues: IReportCashFlow = {};

  const startYearToShow = startDateToShow.getFullYear();
  const startYear = startDateToGenerate.getFullYear();
  const endYear = endDate.getFullYear();

  const currentDate = new Date();

  for (let year = startYear; year <= endYear; year++) {
    const date = new Date(year, 0, 1);
    const dateYear = new Date(date).getFullYear();
    const { periodGraphic, periodTable, periodKey } = getYearlyPeriod(date);

    initialValues[periodKey] = {
      ...defaultValuesReportCashFlow,
      isToShow: dateYear >= startYearToShow && dateYear <= endYear,
      isActualPeriod: currentDate.getFullYear() === year,
      periodGraphic: `${periodGraphic}`,
      periodTable: `${periodTable}`,
    };
  }

  return initialValues;
};

export const createInitialValuesCashFlowReport = (vision: VisionReportEnum, dates: ICreateInitialValuesCashFlow) => {
  switch (vision) {
    case VisionReportEnum.MONTHLY:
      return createInitialValuesCashFlowByMonth(dates);
    case VisionReportEnum.QUARTER:
      return createInitialValuesCashFlowByQuarter(dates);
    case VisionReportEnum.SEMESTER:
      return createInitialValuesCashFlowBySemester(dates);
    case VisionReportEnum.YEARLY:
      return createInitialValuesCashFlowByYear(dates);
    default:
      return createInitialValuesCashFlowByMonth(dates);
  }
};

export const calcAHPercent = (value: number, previousValue: number) => {
  if (value === 0) return 0;
  if (previousValue === 0) return 100;

  const ah = ((value - previousValue) / previousValue) * 100;

  return Number(ah.toFixed(2));
};
```
{% endcode %}

