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

# 🔑 Como acessar máquina com certificado

Adicionar permissão no arquivo

```sh
chmod 600 ARQUIVO
```

> A permissão "600" em sistemas Unix/Linux concede total acesso (leitura e escrita) apenas ao proprietário do arquivo, restringindo totalmente o acesso para grupos e outros usuários.

Acessar á máquina

```sh
ssh -i ARQUIVO usuario@ip
```
