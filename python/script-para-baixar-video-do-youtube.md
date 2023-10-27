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

# 🎥 Script para baixar vídeo do Youtube

<figure><img src="https://miro.medium.com/v2/resize:fit:560/0*zKnCdYeZxzf8ota3.jpg" alt="" height="438" width="700"><figcaption></figcaption></figure>

Com certeza você já teve vontade de baixar um vídeo do YouTube para utilizar em outro lugar ou apenas para poder reproduzir offline. Existem diversos programas ou sites que fazem isso mas quando o número de vídeos começa a aumentar ou precisamos baixar uma _playlist_, talvez seja a hora de utilizar um programa feito por você mesmo. Para isso, existe a biblioteca [PyTube](https://github.com/nficano/pytube).

> _**Atenção**: Este artigo é de caráter educacional e não deve ser utilizado para obter conteúdos protegidos por direitos autorais._

Para os mais ansiosos vamos ver logo como é simples baixar um vídeo usando o PyTube:

## Instalação do PyTube <a href="#abd4" id="abd4"></a>

Como boa prática, utilize sempre um ambiente virtual em todos os seus projetos. Se você não sabe como criar um utilizando o Pipenv, aprenda [aqui](https://github.com/pypa/pipenv)

```sh
pipenv install pytube3
pipenv shell
```

## Download de um vídeo <a href="#a79b" id="a79b"></a>

```python
from pytube import YouTube

VIDEO_URL = 'https://www.youtube.com/watch?v=8EJ3zbKTWQ8&list='
yt = YouTube(VIDEO_URL)

for stream in yt.streams:
    print(stream)
```

## Download de uma playlist <a href="#2f35" id="2f35"></a>

{% code overflow="wrap" %}
```python
from pytube import YouTube, Playlist

PLAYLIST_URL = 'https://www.youtube.com/playlist?list=PLyORnIW1xT6waC0PNjAMj33FdK2ngL_ik'

playlist = Playlist(PLAYLIST_URL)

for url in playlist:
    video = YouTube(url)
    stream = video.streams.get_highest_resolution()
    stream.download(output_path='playlist')
```
{% endcode %}

## Bônus: Download do áudio de um vídeo <a href="#5a4b" id="5a4b"></a>

Podemos, também, baixar apenas um arquivo de áudio:

```python
audio = yt.streams.filter(only_audio=True)[0]
audio.download()
```
