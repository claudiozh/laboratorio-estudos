# 📄 Paginação com prisma

NestJS é uma estrutura para a construção de aplicativos Node.js do lado do servidor eficientes e escalonáveis. Prisma é uma poderosa ferramenta ORM (Mapeamento Objeto-Relacional) que pode ser usada com NestJS para interagir com bancos de dados. Para habilitar a paginação em um aplicativo NestJS usando Prisma, você pode usar as opções skip e take nos métodos prisma.query ou prisma.mutation para limitar o número de registros retornados em uma consulta. Por exemplo, para recuperar a segunda página de uma lista de usuários com tamanho de página 10, você pode usar a seguinte consulta

```typescript
const users = await prisma.query.users({
  skip: 10,
  take: 10
});
```

Aqui está um exemplo de função de paginação simples que pode ser usada para paginar uma lista de dados usando NestJS e Prisma:

```typescript
import PrismaService from '@providers/prisma/prisma.service';
import { PaginatedResult, PaginateFunction, paginator } from '@providers/prisma/paginator';
import { User, Prisma } from '@prisma/client';

const paginate: PaginateFunction = paginator({ perPage: 10 });

@Injectable()
export default class UserRepository {
    constructor(private prisma: PrismaService) {}

    async findMany({ where, orderBy, page }:{
        where?: Prisma.UserWhereInput,
        orderBy?: Prisma.UserOrderByWithRelationInput,
        page?: number,
    }): Promise<PaginatedResult<User>> {
        return paginate(
            this.prisma.user,
            {
                where,
                orderBy,
            },
            {
                page,
            },
        );
    }
}
```

Vamos ver a implementação deste paginador em paginator.ts:

```typescript
export interface PaginatedResult<T> {
  data: T[]
  meta: {
    total: number
    lastPage: number
    currentPage: number
    perPage: number
    prev: number | null
    next: number | null
  }
}

export type PaginateOptions = { page?: number | string, perPage?: number | string }
export type PaginateFunction = <T, K>(model: any, args?: K, options?: PaginateOptions) => Promise<PaginatedResult<T>>

export const paginator = (defaultOptions: PaginateOptions): PaginateFunction => {
  return async (model, args: any = { where: undefined }, options) => {
    const page = Number(options?.page || defaultOptions?.page) || 1;
    const perPage = Number(options?.perPage || defaultOptions?.perPage) || 10;

    const skip = page > 0 ? perPage * (page - 1) : 0;
    const [total, data] = await Promise.all([
      model.count({ where: args.where }),
      model.findMany({
        ...args,
        take: perPage,
        skip,
      }),
    ]);
    const lastPage = Math.ceil(total / perPage);

    return {
      data,
      meta: {
        total,
        lastPage,
        currentPage: page,
        perPage,
        prev: page > 1 ? page - 1 : null,
        next: page < lastPage ? page + 1 : null,
      },
    };
  };
};
```

Exemplo de resultado:

```json
{
    "data": [
      {
          "id": 1,
          "firstName": null,
          "lastName": null,
          "dateOfBirth": null,
          "email": "test@gmail.com",
          "zipCode": null,
          "city": null,
          "avatar": null
      }, 
      ...
    ],
    "meta": {
        "total": 5,
        "lastPage": 1,
        "currentPage": 1,
        "perPage": 10,
        "prev": null,
        "next": null
    }
}
```
