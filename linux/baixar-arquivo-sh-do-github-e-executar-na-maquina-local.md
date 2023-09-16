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

# Baixar arquivo sh do github e executar na máquina local

{% code overflow="wrap" %}
```sh
sh <(wget -qO- https://raw.githubusercontent.com/claudiozh/script-start-project-node/main/start.sh)
```
{% endcode %}
