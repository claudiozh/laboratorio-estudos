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

# 🤡 Closure

Em Go, uma closure é uma função anônima que "captura" uma ou mais variáveis do ambiente externo e pode acessá-las mesmo após a função ter terminado sua execução. Aqui está um exemplo simples de uma closure em Go:

```go
package main

import "fmt"

// A função retorna uma closure que incrementa o valor passado.
func criarIncrementador(incremento int) func() int {
	return func() int {
		incremento++
		return incremento
	}
}

func main() {
	// Cria uma closure que incrementa um contador em 1.
	incrementadorDeUm := criarIncrementador(1)

	// Chama a closure várias vezes para aumentar o contador.
	fmt.Println(incrementadorDeUm()) // 2
	fmt.Println(incrementadorDeUm()) // 3
	fmt.Println(incrementadorDeUm()) // 4

	// Cria outra closure com um incremento diferente.
	incrementadorDeCinco := criarIncrementador(5)

	// O contador é independente para cada closure.
	fmt.Println(incrementadorDeCinco()) // 6
	fmt.Println(incrementadorDeCinco()) // 7
}
```

Neste exemplo, `criarIncrementador` é uma função que retorna uma closure. Essa closure incrementa a variável `incremento` sempre que é chamada e retorna o valor incrementado.

Na função `main`, criamos duas closures usando `criarIncrementador`. Uma delas incrementa o contador em 1 e a outra em 5. Como resultado, cada closure tem seu próprio estado interno independente, e podemos chamar essas closures várias vezes para ver os valores incrementados de acordo com o incremento especificado.

```bash
2
3
4
6
7
```
