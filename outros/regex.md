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

# Regex

### Regex para aceitar somente letras de **\[ a-z e \_ ]** e limitar quantidade de caracteres.

```
/^[a-z_]{0,50}$/
```

### Regex somente letras e números

```
/^[a-zA-Z0-9]+$/
```

### Remover espaços de uma string

```
/\s/g
```

### Remover palavra de uma string

```
/PALAVRA/gi
```

### Remover multiplas palavras de uma string

```
/[PALAVRA_1,PALAVRA_2]/gi
```

### Remover multiplos espaços de uma string e limitar para ficar apenas um

```
\s{1,}
```

### Regex para remover tudo que for diferente de letras (Com ou sem ascentos) e números

```
/[^a-zA-Z0-9\s\u00C0-\u00FF]/g
```

### Regex para checar se uma determinada ocorrência acontece em uma string

```
/(.)\1/
```

* (.) Dê match em qualquer caractere e crie um grupo
* \1 Encontre o caractere que você acabou de encontrar de novo
* Você pode misturar isso com + ou os ranges de repetição {min,max} para fazer qualquer match
* `(.)\1{2}` Ex: 1211: true

### Referências

[dev.to](https://dev.to/ruppysuppy/the-regular-expression-regex-cheat-sheet-you-always-wanted-1c8h?utm\_campaign=Codecon%20Newsletter\&utm\_medium=email\&utm\_source=Revue%20newsletter)
