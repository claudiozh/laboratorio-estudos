---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🔍 Filtros com prisma

Filtrar registros é uma das habilidades essenciais ao trabalhar com bancos de dados SQL. Neste artigo, implementaremos vários exemplos usando NestJS e Prisma para mostrar como filtrar os dados em diferentes casos. Graças a isso, aprenderemos como encontrar com precisão os dados que precisamos de forma rápida e fácil.



Nesta série, frequentemente filtramos os registros fornecendo o valor exato que procuramos.

{% code title="articles.service.ts" %}
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../database/prisma.service';
import { ArticleNotFoundException } from './article-not-found.exception';
 
@Injectable()
export class ArticlesService {
  constructor(private readonly prismaService: PrismaService) {}
 
  // ...
  
  async getById(id: number) {
    const article = await this.prismaService.article.findUnique({
      where: {
        id,
      },
    });
    
    if (!article) {
      throw new ArticleNotFoundException(id);
    }
    
    return article;
  }
}
```
{% endcode %}

Além de procurar uma linha com um valor específico, podemos utilizar vários operadores de filtragem. Um dos mais simples é contains .

{% code title="articles.service.ts" %}
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../database/prisma.service';
 
@Injectable()
export class ArticlesService {
  constructor(private readonly prismaService: PrismaService) {}
 
  searchByText(query: string) {
    return this.prismaService.article.findMany({
      where: {
        content: {
          contains: query,
          mode: 'insensitive',
        },
      },
    });
  }
 
  // ...
}
```
{% endcode %}

{% hint style="info" %}
Graças à adição de mode : '**insensitive**' , nossa pesquisa não diferencia maiúsculas de minúsculas.
{% endhint %}

Vamos permitir que os usuários pesquisem os artigos enviando um parâmetro de consulta.

{% code title="articles-search-params.dto.ts" %}
```typescript
import { IsNotEmpty, IsOptional, IsString } from 'class-validator';
 
export class ArticlesSearchParamsDto {
  @IsOptional()
  @IsNotEmpty()
  @IsString()
  textSearch?: string | null;
}

```
{% endcode %}

Agora podemos usá-lo em nosso controlador.

{% code title="articles.controller.ts" %}
```typescript
import { Controller, Get, Query } from '@nestjs/common';
import { ArticlesService } from './articles.service';
import { ArticlesSearchParamsDto } from './dto/articles-search-params.dto';
 
@Controller('articles')
export default class ArticlesController {
  constructor(private readonly articlesService: ArticlesService) {}
 
  @Get()
  getAll(@Query() { textSearch }: ArticlesSearchParamsDto) {
    if (textSearch) {
      return this.articlesService.searchByText(textSearch);
    }
    
    return this.articlesService.getAll();
  }
 
  // ...
}
```
{% endcode %}

Graças a isso, os usuários agora podem fazer solicitações GET que filtram os artigos pelo conteúdo.

![](http://wanago.io/wp-content/uploads/2023/12/Screenshot-from-2023-12-16-21-52-17.png)

### Combinando múltiplas condições de filtragem

Podemos aplicar várias condições de pesquisa em uma única consulta.

### Usando o operador OR

Por exemplo, vamos pesquisar artigos que contenham um determinado trecho de texto no título **ou** no conteúdo.

{% code title="articles.service.ts" %}
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../database/prisma.service';
 
@Injectable()
export class ArticlesService {
  constructor(private readonly prismaService: PrismaService) {}
 
  searchByText(query: string) {
    return this.prismaService.article.findMany({
      where: {
        OR: [
          {
            content: {
              contains: query,
              mode: 'insensitive',
            },
          },
          {
            title: {
              contains: query,
              mode: 'insensitive',
            },
          },
        ],
      },
    });
  }
 
  // ...
}
```
{% endcode %}

Acima, estamos passando uma série de condições para o operador OR . Graças a isso, incluímos o artigo se o conteúdo ou o título corresponderem à consulta.

### Usando o operador AND

Também podemos combinar artigos que cumpram múltiplas condições. Vamos permitir que os usuários obtenham os artigos com base no número de votos positivos.

{% code title="articles-search-params.dto.ts" %}
```typescript
import { IsNotEmpty, IsNumber, IsOptional, IsString } from 'class-validator';
import { Type } from 'class-transformer';
 
export class ArticlesSearchParamsDto {
  @IsOptional()
  @IsNotEmpty()
  @IsString()
  textSearch?: string | null;
 
  @Type(() => Number)
  @IsOptional()
  @IsNumber()
  upvotesGreaterThan?: number | null;
}
```
{% endcode %}

Precisamos que o operador AND exija que os artigos cumpram mais de uma condição.

{% code title="articles.service.ts" %}
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../database/prisma.service';
 
@Injectable()
export class ArticlesService {
  constructor(private readonly prismaService: PrismaService) {}
 
  search(textSearch: string, upvotesGreaterThan: number) {
    return this.prismaService.article.findMany({
      where: {
        AND: [
          {
            content: {
              contains: textSearch,
              mode: 'insensitive',
            },
          },
          {
            upvotes: {
              gt: upvotesGreaterThan,
            },
          },
        ],
      },
    });
  }
 
  // ...
}
```
{% endcode %}

Com a abordagem acima, um determinado artigo precisa ter um texto específico em seu conteúdo e um determinado número de votos positivos.

Podemos dar um passo adiante e combinar os operadores AND e OR .

{% code title="articles.services.ts" %}
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../database/prisma.service';
 
@Injectable()
export class ArticlesService {
  constructor(private readonly prismaService: PrismaService) {}
 
  search(textSearch: string, upvotesGreaterThan: number) {
    return this.prismaService.article.findMany({
      where: {
        AND: [
          {
            OR: [
              {
                content: {
                  contains: textSearch,
                  mode: 'insensitive',
                },
              },
              {
                title: {
                  contains: textSearch,
                  mode: 'insensitive',
                },
              },
            ],
          },
          {
            upvotes: {
              gt: upvotesGreaterThan,
            },
          },
        ],
      },
    });
  }
  
  // ...
}
```
{% endcode %}

No entanto, há um problema com essa abordagem. Devemos ter os seguintes casos:

* o usuário não forneceu nenhum parâmetro de pesquisa,
* eles forneceram apenas o parâmetro de pesquisa de texto,
* eles forneceram apenas o parâmetro upvotes,
* eles forneceram o parâmetro de pesquisa de texto e o parâmetro de votos positivos.

Para fazer isso, precisamos ser um pouco criativos.

{% code title="articles.services.ts" %}
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../database/prisma.service';
import { Prisma } from '@prisma/client';
import { ArticlesSearchParamsDto } from './dto/articles-search-params.dto';
 
@Injectable()
export class ArticlesService {
  constructor(private readonly prismaService: PrismaService) {}
 
  search({ textSearch, upvotesGreaterThan }: ArticlesSearchParamsDto) {
    const searchInputs: Prisma.ArticleWhereInput[] = [];
 
    if (textSearch) {
      searchInputs.push({
        OR: [
          {
            content: {
              contains: textSearch,
              mode: 'insensitive',
            },
          },
          {
            title: {
              contains: textSearch,
              mode: 'insensitive',
            },
          },
        ],
      });
    }
 
    if (typeof upvotesGreaterThan === 'number') {
      searchInputs.push({
        upvotes: {
          gt: upvotesGreaterThan,
        },
      });
    }
 
    if (!searchInputs.length) {
      return this.getAll();
    }
 
    if (searchInputs.length === 1) {
      return this.prismaService.article.findMany({
        where: searchInputs[0],
      });
    }
 
    return this.prismaService.article.findMany({
      where: {
        AND: searchInputs,
      },
    });
  }
 
  getAll() {
    return this.prismaService.article.findMany();
  }
 
  // ...
}
```
{% endcode %}

Com essa abordagem, tratamos os parâmetros de pesquisa de maneira diferente com base em quantos deles o usuário forneceu:

* se eles não fornecerem nenhum, chamamos o método getAll ,
* se eles fornecerem um único parâmetro, não usaremos o operador AND ,
* usamos o operador AND somente se o usuário forneceu mais de um parâmetro de pesquisa.

Esta solução mantém nosso controlador limpo e simples porque só precisamos chamar o método **de pesquisa**.

{% code title="articles.controller.ts" %}
```typescript
import { Controller, Get, Query } from '@nestjs/common';
import { ArticlesService } from './articles.service';
import { ArticlesSearchParamsDto } from './dto/articles-search-params.dto';
 
@Controller('articles')
export default class ArticlesController {
  constructor(private readonly articlesService: ArticlesService) {}
 
  @Get()
  getAll(@Query() searchParams: ArticlesSearchParamsDto) {
    return this.articlesService.search(searchParams);
  }
 
  // ...
}
```
{% endcode %}

### Filtrando relacionamentos

Também podemos usar o Prisma para filtrar os registros relacionados. Vamos permitir que os usuários filtrem os artigos pelo nome da categoria.

{% code title="articles-search-params.dto.ts" %}
```typescript
import { IsNotEmpty, IsNumber, IsOptional, IsString } from 'class-validator';
import { Type } from 'class-transformer';
 
export class ArticlesSearchParamsDto {
  @IsOptional()
  @IsNotEmpty()
  @IsString()
  textSearch?: string | null;
 
  @Type(() => Number)
  @IsOptional()
  @IsNumber()
  upvotesGreaterThan?: number | null;
 
  @IsOptional()
  @IsNotEmpty()
  @IsString()
  categoryName?: string | null;
}
```
{% endcode %}

Um único artigo pode ter várias categorias. Vamos usar o operador some para obter artigos onde pelo menos uma categoria tenha um nome específico.

```prisma
this.prismaService.article.findMany({
  where: {
    categories: {
      some: {
        name: categoryName,
      },
    },
  }
});
```

{% hint style="info" %}
A documentação oficial menciona mais operadores que podem ser úteis em relacionamentos, como each , ou none .
{% endhint %}

Vamos adicionar este filtro ao nosso método **de pesquisa**.

{% code title="articles.service.ts" %}
```typescript
import { BadRequestException, Injectable } from '@nestjs/common';
import { PrismaService } from '../database/prisma.service';
import { CreateArticleDto } from './dto/create-article.dto';
import { Prisma } from '@prisma/client';
import { PrismaError } from '../database/prisma-error.enum';
import { ArticleNotFoundException } from './article-not-found.exception';
import { UpdateArticleDto } from './dto/update-article.dto';
import { ArticlesSearchParamsDto } from './dto/articles-search-params.dto';
 
@Injectable()
export class ArticlesService {
  constructor(private readonly prismaService: PrismaService) {}
 
  search({
    textSearch,
    upvotesGreaterThan,
    categoryName,
  }: ArticlesSearchParamsDto) {
    const searchInputs: Prisma.ArticleWhereInput[] = [];
 
    if (categoryName) {
      searchInputs.push({
        categories: {
          some: {
            name: categoryName,
          },
        },
      });
    }
 
    // ...
 
    return this.prismaService.article.findMany({
      where: {
        AND: searchInputs,
      },
    });
  }
 
  // ...
}
```
{% endcode %}

### Negando filtros

Outra técnica útil nos permite negar certos filtros. Por exemplo, vamos permitir que os usuários filtrem os artigos escritos por um autor com um nome específico.

{% code title="articles-search-params.dto.ts" %}
```typescript
import { IsNotEmpty, IsNumber, IsOptional, IsString } from 'class-validator';
import { Type } from 'class-transformer';
 
export class ArticlesSearchParamsDto {
  @IsOptional()
  @IsNotEmpty()
  @IsString()
  textSearch?: string | null;
 
  @Type(() => Number)
  @IsOptional()
  @IsNumber()
  upvotesGreaterThan?: number | null;
 
  @IsOptional()
  @IsNotEmpty()
  @IsString()
  categoryName?: string | null;
 
  @IsOptional()
  @IsNotEmpty()
  @IsString()
  authorNameToAvoid?: string;
}
```
{% endcode %}

Para negar um filtro específico, podemos usar o operador **NOT** .

```prisma
this.prismaService.article.findMany({
  where: {
    NOT: {
      author: {
        name: authorNameToAvoid,
      },
    },
  }
});
```

Podemos adicioná-lo ao nosso método **de pesquisa**.

{% code title="articles.service.ts" %}
```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../database/prisma.service';
import { Prisma } from '@prisma/client';
import { ArticlesSearchParamsDto } from './dto/articles-search-params.dto';
 
@Injectable()
export class ArticlesService {
  constructor(private readonly prismaService: PrismaService) {}
 
  search({
    textSearch,
    upvotesGreaterThan,
    categoryName,
    authorNameToAvoid,
  }: ArticlesSearchParamsDto) {
    const searchInputs: Prisma.ArticleWhereInput[] = [];
 
    if (authorNameToAvoid) {
      searchInputs.push({
        NOT: {
          author: {
            name: authorNameToAvoid,
          },
        },
      });
    }
    
    // ...
 
    return this.prismaService.article.findMany({
      where: {
        AND: searchInputs,
      },
    });
  }
  
  // ...
}
```
{% endcode %}

### Resumo

Neste artigo, aprendemos como filtrar registros ao usar o Prisma implementando um recurso de pesquisa. Ao fazer isso, aprendemos como implementar a filtragem condicional ao usar o operador AND . Embora tenhamos incluído vários cenários da vida real, o Prisma menciona outros operadores úteis em [sua documentação oficial](https://www.prisma.io/docs/orm/reference/prisma-client-reference#filter-conditions-and-operators) . No entanto, analisar os exemplos deste artigo pode lhe dar uma compreensão sólida do que o Prisma pode fazer.

### Referência

[https://wanago.io/2023/12/18/api-nestjs-prisma-sql-filtering/](https://wanago.io/2023/12/18/api-nestjs-prisma-sql-filtering/)
