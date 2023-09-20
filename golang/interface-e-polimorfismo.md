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

# 😥 Interface e Polimorfismo

{% code overflow="wrap" %}
```go
package main

import "fmt"

type Veiculo interface {
	getNome() string
	getVelocidadeMaxima() float64
}

type Carro struct {
	nome               string
	cavalosDeForca     int
	quantidadeDePortas int
}

type Moto struct {
	nome                  string
	cilindradas           int
	possuiPartidaEletrica bool
}

func (c Carro) getNome() string {
	return c.nome
}

func (c Carro) getVelocidadeMaxima() float64 {
	return float64(c.cavalosDeForca * 50)
}

func (m Moto) getNome() string {
	return m.nome
}

func (m Moto) getVelocidadeMaxima() float64 {
	return float64(m.cilindradas * 2)
}

func imprimirDados(v Veiculo) {
	fmt.Printf("Nome: %s\nVelocidade Máxima: %.2f\n\n", v.getNome(), v.getVelocidadeMaxima())
}

func main() {
	carro := Carro{
		nome:               "Celta",
		cavalosDeForca:     80,
		quantidadeDePortas: 4,
	}

	moto := Moto{
		nome:                  "CG",
		cilindradas:           150,
		possuiPartidaEletrica: true,
	}

	imprimirDados(carro)
	imprimirDados(moto)
}
```
{% endcode %}
