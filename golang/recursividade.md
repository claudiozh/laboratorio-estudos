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

# 🌪 Recursividade

```go
package main

import "fmt"

// Função recursiva para calcular o fatorial
func fatorial(n int) int {
	if n == 0 {
		return 1
	}
	return n * fatorial(n-1)
}

func main() {
	numero := 5 // O número para o qual você deseja calcular o fatorial
	resultado := fatorial(numero)
	fmt.Printf("O fatorial de %d é %d\n", numero, resultado)
}
```
