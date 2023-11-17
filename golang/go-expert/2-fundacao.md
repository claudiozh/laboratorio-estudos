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

# ✏ 2 - Fundação

## Declaração e atribuição

Em Go, ao declarar uma variável ela automaticamente é inferido um valor inicial (Conhecido como valor zero)

```go
package main

import "fmt"

var (
	a string
	b bool
	c int
	d float64
)

func main() {
	fmt.Println(a) // ""
	fmt.Println(b) // false
	fmt.Println(c) // 0 
	fmt.Println(d) // 0
}
```

Também é possível declarar e atribuir valores

```go
package main

import "fmt"

var (
	a string  = "Claudio"
	b bool    = true
	c int     = 25
	d float64 = 1.25
)

func main() {
	fmt.Println(a) // "Claudio"
	fmt.Println(b) // true
	fmt.Println(c) // 25
	fmt.Println(d) // 1.25
}

```

## Criação de tipos

```go
package main

import "fmt"

type ID string

var a ID = "Meu ID"

func main() {
    fmt.Println(a)
}
```

## Importando fmt e tipagem

```go
package main

import "fmt"

var a string = "Hello, World"

func main() {
	fmt.Printf("\nO valor de a é: %v", a)
	fmt.Printf("\nO tipo de a é: %T", a)
}
```

## Percorrendo arrays

<pre class="language-go"><code class="lang-go">package main

import "fmt"

func main() <a data-footnote-ref href="#user-content-fn-1">{</a>
	var meuArray [3]string

	meuArray[0] = "Claudio"
	meuArray[1] = "Rodrigo"
	meuArray[2] = "Medeiros"

	lastIndex := len(meuArray) - 1

	fmt.Println(meuArray[lastIndex])

	for i, v := range meuArray {
		fmt.Printf("O Valor do índice é %v e o valor é %v\n", i, v)
	}
}
</code></pre>

## Slices

```go
package main

import "fmt"

func main() {
	s := []int{1, 2, 3, 4, 5}

	fmt.Printf("len=%d capacity=%d %v\n", len(s), cap(s), s)

	fmt.Printf("len=%d capacity=%d %v\n", len(s[:0]), cap(s[:0]), s[:0])

	fmt.Printf("len=%d capacity=%d %v\n", len(s[:4]), cap(s[:4]), s[:4])

	fmt.Printf("len=%d capacity=%d %v\n", len(s[2:]), cap(s[2:]), s[2:])

	s = append(s, 6)

	fmt.Printf("len=%d capacity=%d %v\n", len(s), cap(s), s)

	s = append(s, 7)

	fmt.Printf("len=%d capacity=%d %v\n", len(s), cap(s), s)
}
```

## Maps

```go
package main

import "fmt"

func main() {
	salarios := map[string]int{
		"Claudio":  100,
		"Rodrigo":  150,
		"Medeiros": 120,
	}

	// Outras forma de declarar um map
	// salarios := map[string]int{}
	// salarios := make(map[string]int)

	fmt.Println(salarios["Claudio"])

	delete(salarios, "Claudio")

	salarios["Marcos"] = 160

	fmt.Println(salarios["Marcos"])

	for nome, salario := range salarios {
		fmt.Printf("O salário de %s é %d\n", nome, salario)
	}

	// Range com blank identifier
	for _, salario := range salarios {
		fmt.Printf("O salário é de %d\n", salario)
	}
}
```

## Funções

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	value, err := sum(0, 0)

	if err != nil {
		fmt.Println(err)
		return
	}

	fmt.Println(value)
}

func sum(a, b int) (int, error) {
	result := a + b

	if result <= 0 {
		return 0, errors.New("Valores inválidos")
	}

	return result, nil
}
```

## Funções variadicas

São funções com uma quantidade ilimitada de parâmetros do mesmo tipo

```go
package main

import "fmt"

func main() {
	fmt.Println(sum(2, 3, 4, 5))
}

func sum(numeros ...int) int {
	total := 0

	for _, numero := range numeros {
		total += numero
	}

	return total
}
```

## Closure

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

## Structs

```go
package main

import "fmt"

type Cliente struct {
	Nome  string
	Idade int
	Ativo bool
}

func main() {
	claudio := Cliente{
		Nome:  "Claudio",
		Idade: 25,
		Ativo: true,
	}

	fmt.Println("Nome: ", claudio.Nome)
	fmt.Println("Idade: ", claudio.Idade)
	fmt.Println("Ativo: ", claudio.Ativo)
}
```

## Composição de Structs

```go
package main

import "fmt"

type Endereco struct {
	Logradouro string
	Numero     int
	Cidade     string
	Estado     string
}

type Cliente struct {
	Nome  string
	Idade int
	Ativo bool
	Endereco
}

func main() {
	claudio := Cliente{
		Nome:  "Claudio",
		Idade: 25,
		Ativo: true,
	}

	endereco := Endereco{
		Logradouro: "Rua José Oliveira",
		Numero:     25,
		Cidade:     "Portalegre",
		Estado:     "RN",
	}

	claudio.Endereco = endereco

	fmt.Println(claudio)
}
```

## Métodos em Structs

```go
package main

import "fmt"

type Endereco struct {
	Logradouro string
	Numero     int
	Cidade     string
	Estado     string
}

type Cliente struct {
	Nome  string
	Idade int
	Ativo bool
	Endereco
}

func (c *Cliente) Desativar() {
	c.Ativo = false
	fmt.Printf("O cliente %s foi desativado\n", c.Nome)
}

func main() {
	claudio := Cliente{
		Nome:  "Claudio",
		Idade: 25,
		Ativo: true,
	}

	endereco := Endereco{
		Logradouro: "Rua José Oliveira",
		Numero:     25,
		Cidade:     "Portalegre",
		Estado:     "RN",
	}

	claudio.Endereco = endereco

	claudio.Desativar()

	fmt.Printf("O novo status dele é %v", claudio.Ativo)
}
```

## Interfaces

Um ponto bastante importante é que as interfaces em GO são somente para declaração de métodos.

{% code overflow="wrap" %}
```go
package main

import "fmt"

type Pessoa interface {
	Desativar()
}

func Desativacao(p Pessoa) {
	p.Desativar()
}

type Cliente struct {
	Nome  string
	Idade int
	Ativo bool
}

func (c *Cliente) Desativar() {
	c.Ativo = false
}

type Empresa struct {
	RazaoSocial  string
	NomeFantasia string
	Ativo        bool
}

func (e *Empresa) Desativar() {
	e.Ativo = false
}

func main() {
	claudio := Cliente{
		Nome:  "Claudio",
		Idade: 25,
		Ativo: true,
	}

	empresa := Empresa{
		RazaoSocial:  "Mercantil Oliveira LTDA",
		NomeFantasia: "Mercantil Oliveira",
		Ativo:        true,
	}

	Desativacao(&claudio)
	Desativacao(&empresa)

	fmt.Printf("Nome: %s, Idade: %d, Situação: %v\n", claudio.Nome, claudio.Idade, claudio.Ativo)
	fmt.Printf("Empresa: %s, Situação: %v\n", empresa.NomeFantasia, empresa.Ativo)
}
```
{% endcode %}

## Ponteiros

```go
package main

import "fmt"

func main() {
	a := 10
	var ponteiro *int = &a

	fmt.Println(ponteiro)

	*ponteiro = 20

	fmt.Println(a)
	fmt.Println(*ponteiro)
	fmt.Println(&a)
	fmt.Println(ponteiro)
}
```

[^1]: Teste
