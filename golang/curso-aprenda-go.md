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

# 🦫 Curso: Aprenda Go

### Operadores

<pre><code><strong>:= Declaração
</strong>= Atribuição
</code></pre>

### Palavras reservadas

```
break        default      func         interface    select
case         defer        go           map          struct
chan         else         goto         package      switch
const        fallthrough  if           range        type
continue     for          import       return       var

```

### Tipagem

* Tipos em Go são extremamente importantes. (Veremos mais quando chegarmos em métodos e interfaces.)
* Tipos em Go são estáticos.
* Ao declarar uma variável para conter valores de um certo tipo, essa variável só poderá conter valores desse tipo.
* O tipo pode ser deduzido pelo compilador:
  * x := 10
  * var y = "a tia do batman"
* Ou pode ser declarado especificamente:
  * var w string = "isso é uma string"
  * var z int = 15
  * na declaração var z int com package scope, atribuição z = 15 no codeblock (somente)
* Tipos de dados primitivos: disponíveis na linguagem nativamente como blocos básicos de construção
  * int, string, bool
* Tipos de dados compostos: são tipos compostos de tipos primitivos, e criados pelo usuário
  * slice, array, struct, map
* O ato de definir, criar, estruturar tipos compostos chama-se composição. Veremos muito disso futuramente.

### Valor zero

O que é valor zero? É o valor default de uma variável quando criado e não inicializada

Os zeros:

* ints: 0
* floats: 0.0
* booleans: false
* strings: ""
* pointers, functions, interfaces, slices, channels, maps: nil

### Exercicios

[https://docs.google.com/forms/d/e/1FAIpQLScmMK7rjqj9SF2qTaN4Vg6mQX19YWqop7WRSfHjxZT-xbqdVQ/viewform](https://docs.google.com/forms/d/e/1FAIpQLScmMK7rjqj9SF2qTaN4Vg6mQX19YWqop7WRSfHjxZT-xbqdVQ/viewform)

### Referências

[https://www.youtube.com/@AprendaGo](https://www.youtube.com/@AprendaGo)
