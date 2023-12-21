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

# 👾 Comandos uteis

## Git

Algumas notas e dicas sobre git e github

#### Mostrando logs <a href="#mostrando-logs" id="mostrando-logs"></a>

git log

#### Corrigir mensagem do ultimo commit <a href="#corrigir-mensagem-do-ultimo-commit" id="corrigir-mensagem-do-ultimo-commit"></a>

​[https://youtu.be/6OokP-NE49k?si=Ucqk0UpILl72Bg5u\&t=187](https://youtu.be/6OokP-NE49k?si=Ucqk0UpILl72Bg5u\&t=187)​git commit -m "Mensagemd de commit correta" --amend

#### Mesclando commits antes de fazer o pull <a href="#mesclando-commits-antes-de-fazer-o-pull" id="mesclando-commits-antes-de-fazer-o-pull"></a>

​[https://youtu.be/6OokP-NE49k?si=qGTV7csc6QrkDZfh\&t=230](https://youtu.be/6OokP-NE49k?si=qGTV7csc6QrkDZfh\&t=230)​Existe mais de uma forma de fazer isso

**Usando rest soft**

HEAD Para remover os commits a partir da cabeca e \~2 para remover os dois ultimos commits.Este comando deve desfazer apenas os commits, mas não irá remover as alterações no arquivos, permitindo que voce possa dar um git commitgit reset --soft HEAD\~2

**Usando o modo interativo e rebase**

​[https://youtu.be/6OokP-NE49k?si=rq28edXqmHmNRe8I\&t=373](https://youtu.be/6OokP-NE49k?si=rq28edXqmHmNRe8I\&t=373)​git rebase -i HEAD\~3

#### Removendo um arquivo do staged <a href="#removendo-um-arquivo-do-staged" id="removendo-um-arquivo-do-staged"></a>

git reset -- fileName.txt

#### Removendo todos os arquivos do staged <a href="#removendo-todos-os-arquivos-do-staged" id="removendo-todos-os-arquivos-do-staged"></a>

git reset --

#### Removendo um arquivo do staged usando a opção interativa <a href="#removendo-um-arquivo-do-staged-usando-a-opcao-interativa" id="removendo-um-arquivo-do-staged-usando-a-opcao-interativa"></a>

git add -i

* Vai abrir um terminal interativo com algumas opções, escolha a opção **3 - revert**
* Vai listar todos os arquivos que estão no staged e você pode ir digitando apenas o número dos arquivos que não deseja comitar no momento
* Ao concluir pressiona **ENTER** para voltar ao menu anterior
* Caso tenha escolhido um arquivo errado e precise adicionar o arquivo no staged escolha a opção **2 - Update** e novamente escolha o número do arquivo que deseja adicionar
* Quando concluir escolhe **quit** para sair

#### Apagando modificações com git reset <a href="#apagando-modificacoes-com-git-reset" id="apagando-modificacoes-com-git-reset"></a>

Cuidado ao utilizar --hard você ira remover tanto os commits quanto as modificações nos arquivosgit reset --hard HEAD\~1

#### Documentei até <a href="#documentei-ate" id="documentei-ate"></a>

​[https://youtu.be/6OokP-NE49k?si=HHoIOciCNKRKr4WF\&t=1012](https://youtu.be/6OokP-NE49k?si=HHoIOciCNKRKr4WF\&t=1012)​

#### Videos Fabio Akita <a href="#videos-fabio-akita" id="videos-fabio-akita"></a>

