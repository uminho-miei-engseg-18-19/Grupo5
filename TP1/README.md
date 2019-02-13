# Trabalho Prático 0 - 11/Fev/2019



## 1. Números aleatórios/pseudoaleatórios



### Pergunta P1.1

Para responder a esta questão, torna-se necessário evidenciar a diferença entre a geração de números pseudoaleatórios com o recurso ao dispositivo `/dev/random` e ao `/dev/urandom`. Estes são ficheiros especiais do sistema *Linux* que têm acesso ao ruído presente no sistema operativo, utilizando a entropia disponível para gerar *bytes* ou uma *seed* pseudoaleatórios, respetivamente.

Por um lado, o dispositivo`/dev/random` requer espera pelo resultado (*bytes*), pois utiliza uma *entropy pool*. Assim, este mecanismo pode bloquear enquanto não for possível gerar dados aleatórios suficientes. Contudo, a realização de chamadas ao sistema operativo permite aumentar a entropia disponível e, consequentemente, possibilitar a geração da quantidade de *bytes* solicitada. Faz sentido utilizar este dispositivo quando são exigidos maiores níveis de qualidade na aleatoriedade.

Por outro lado, o dispositivo `/dev/urandom` não bloqueia devido à reduzida entropia disponível. Este mecanismo retorna valores independentemente de existir entropia suficiente, caso em que, teoricamente, estes são vulneráveis a ataques criptográficos. Neste caso, a entropia apenas é utilizada para gerar uma *seed*, com qualidade superior ou inferior (de acordo com a entropia disponível). De seguida, os *bytes* do resultado são gerados por um *pseudorandom number generator*.

O comando apresentado no início (`openssl rand -base64 1024`) permite gerar *bytes* pseudoaleatórios, após fornecer, uma vez, uma *seed* ao *pseudorandom number generator*, e codifica os mesmos em *base64*. É utilizado o ficheiro `$HOME/.rnd` ou `.rnd` (poderiam ainda considerar-se ficheiros adicionais passados como argumento) para gerar a *seed*.

Os tempos de execução obtidos para cada um dos comandos propostos são apresentados na tabela que se segue:

|                 Comando utilizado                  | Tempo de resolução |
| :------------------------------------------------: | :----------------: |
|            `openssl rand -base64 1024`             |     0.024 seg      |
|  `head -c 32 /dev/random \| openssl enc -base64`   |     0.010 seg      |
|  `head -c 64 /dev/random \| openssl enc -base64`   |     0.011 seg      |
| `head -c 1024 /dev/random \| openssl enc -base64`  |   11 min 23 seg    |
| `head -c 1024 /dev/urandom \| openssl enc -base64` |     0.151 seg      |

Através dos resultados obtidos, verifica-se que na geração de números pseudoaleatórios de tamanhos reduzidos (32 e 64 *bits*) não se evidencia o reduzido de desempenho na utilização do dispositivo `/dev/random`. Contudo, quando se solicita a geração de um número pseudoaleatório de tamanho superior, verifica-se uma grande diferença de desempenho em relação à utilização do `/dev/urandom` (11 minutos e 23 segundos em relação a menos de 1 segundo). Salienta-se ainda um desempenho ligeiramente superior, em relação à utilização do dispositivo `/dev/urandom`, na implementação do *openssl*.



### Pergunta P1.2

O projeto [haveged](http://www.issihosts.com/haveged/index.html) é uma adaptação do algoritmo [HAVEGE](http://www.irisa.fr/caps/projects/hipsor/), que pretende implementar um gerador de números aleatórios imprevisível, através da exploração de modificações dos estados internos e voláteis do *hardware* como fonte de incerteza. Este *daemon* foi criado com o objetivo de remediar condições de baixa entropia nos dispositivos de aleatoriedade *Linux*. Assim, espera-se que a instalação deste tenha um impacto positivo no desempenho dos geradores de números pseudoaleatórios previamente utilizados.

Após a instalação do *daemon* **haveged** e repetição dos comandos indicados na pergunta anterior, obtiveram-se as seguintes medições dos tempos de execução:

|                 Comando utilizado                  | Tempo de resolução |
| :------------------------------------------------: | :----------------: |
|            `openssl rand -base64 1024`             |     0.011 seg      |
|  `head -c 32 /dev/random \| openssl enc -base64`   |     0.010 seg      |
|  `head -c 64 /dev/random \| openssl enc -base64`   |     0.011 seg      |
| `head -c 1024 /dev/random \| openssl enc -base64`  |     0.109 seg      |
| `head -c 1024 /dev/urandom \| openssl enc -base64` |     0.013 seg      |

Conforme esperado, os tempos de execução diminuíram, de forma geral, com especial relevância para o gerador que se baseia no dispositivo `/dev/random`, com o tamanho de 1024 *bits* (passou de 11 minutos e 23 segundos para menos de 1 segundo). Relativamente aos tamanhos inferiores (32 e 64 *bits*), não se verificou melhoria de performance, uma vez que estes comprimentos são demasiado reduzidos para a diferença ser visível.



### Pergunta P1.3

O programa foi executado através do comando `python generateSecret-app.py 1024`, tendo dado origem ao seguinte *output*:

```
npaZnOORSpWsz4KlFbpIE8E5AnzWRX70gU3n8seafJI4ISqYWsV2wCX13eJl2cUvADWhzaGyiTTy8W9hhcwoXLdNgJlshP4Tb89PlnMELxte20M0RcKcy0IS0j376x7Jqf37Td4Jgzs6TnTsacFbv3HRC8j8Ni7K8YbCcCX129QuTD1KGPXVLt1UpWysNtSVgOESRimHtBk2tX7anHwNVA0X3wOVyzR5iUl5jLcMLkG0n0vBFS5BBycliN0G6EYbNlnhnfWBpu4XZdn0iTQok45Ns3tR0JaTLJLsrlGv9tyU38Q4LmtWvoT0yV4x6ipfmH6issoSozontfrm8B4RNWJXSbHgDRsAkaJLou2QTqekWJZzZSe1YkKjPIrNBJnDUuQqCNleydMcakqbiaVD4MbKwLwzZxQ8Egr4qQGnEbtE3M2mf807XfYfLICIl1ZIpGYSgRewduQDYQOumAWs9x1KoNe6fGwamOdjV6spU2cKGPVA3hY7VshKsCA4EVeHrE6hCMITqys38TYeb7xo7AKadodXbOmGtIRSgdHTArCgnOkqwrliEtRhdJgSrHK9HpLwjfXKOucKDLfD9xtsTi2MCoGJKeHlX1T6bov8enfzelHlVMTLd7gVfA3jUcKQWlQnW1eCoQkLD5jMLDXeXuMqzPsv7wPvQU5lcPFrVIdZ07rwCsCwuDK8tmeK06hcTBD31QXnXeRl97KUaxapE58BZPeMhiUoEhfLBo4pZLgH89PsPGAR12MlyUSrKjzd0D8l2jTlA1cN4Ydkyr6KzDnlgXTyvuAh2vNpYXaUVANAF1Rjdd4zgoPDHkgJPanDKEfywCWijrVJHyepjndXWum30Z2Z8kmSlCAWBSQpLX28LHpTtT7jZ9nCR75ATTFO33QBWhV5C1h4qyKKAOnuKMN873SfN4vr0TBbrfF9rxVYKLBqHRrGpr16NQfvgmJ77Pf5cAc0QIc5mv10EhJGlpQwW4mpmHoK9pVDickQzCQbHG9ggPnZr7qBElL3yY33

```

Verifica-se que, de facto, o *output* gerado apenas contém letras e números. Isto deve-se à implementação da função `generateSecret` do módulo `shamirsecret`. Primeiramente, o algoritmo começa por gerar uma sequência de *bytes* pseudoaleatórios com o comprimento em falta no segredo (`s`). De seguida, é avaliado se cada um dos *bytes* é uma letra (`string.ascii_letters`) ou dígito (`string.digits`) e se o comprimento final do segredo ainda não foi atingido, caso em que se concatena o respetivo *byte* ao segredo. Enquanto o comprimento solicitado para o segredo não for atingido, o processo referido anteriormente é repetido.

O código descrito a cima é apresentado de seguida:

```python
def generateRandomData(length):
    return os.urandom(length)

def generateSecret(secretLength):
	l = 0
	secret = ""
	while (l < secretLength):
		s = utils.generateRandomData(secretLength - l)
		for c in s:
			if (c in (string.ascii_letters + string.digits) and l < secretLength): # printable character
				l += 1
				secret += c
	return secret
```

Para não limitar o *output* a letras e dígitos realizaram-se as seguintes alterações:

```
import base64
def generateSecret(secretLength):
	return base64.b64encode(utils.generateRandomData(secretLength))
```

De facto, já não se limita a que apenas os *bytes* que representem letras e dígitos sejam considerados. Assim, torna-se necessário transformar os *bytes* resultantes em *bytes* imprimíveis, realizado através da função `base64.b64encode`. Este facto pode ser testado executando o programa com a nova função, através do qual se obtém o seguinte *output*:

```
$ python generateSecret-app.py 1024
6w49b2/mpSvWjJVn3Hxdhe8IJmzjbiRIYafIVzQ43Vm3axA6QEy+VCdN0q6e+K6NH4u6E22kEci8H1cH6gxy9JVXKyf2Q5b9/1yqkfBTi7hn8Yo6vJx1coytLWRqMooe+jRfn6g0OH8D8GIry+B9AooYjZSyiPbVYXLjHin4U75UH+MyEyOv1fzvmJGr0fModImzh1IH98yNIm+tNMN54zOyKYPBokoTdU6u3k+YozCt0sHvHpbLoD+QBHInlLa5S2J4hRYS8Xq5fOqBczGD2yWowNxy7wxFQRjrTkUEyPLc1UDx1YAdOdt4fg/mPEOkD4IC6dWt4/W1a7HJGHic/pEFYBkwUM4Z5nMNbhEV3lEHw9zMwvN8DZws8c/YksMaG9BxMqLpMB0Zxb0MsKUrFOgDIBIXrEBtYM1JZ0V7zk42bXRiLc1RlF8PnfNVYK+s+MUaQ9nYedzvJNMyX7B0UEECcD+hqjr5yFdnIVmtMtbui3ROyzQktT+HYEm+J+0i34J+hjXpxPBzjcpx48Sgqw9fqn5Py9hxWq+xybJGTQ+7Fp7JCP1pHEYc7Jb6yAxc88JXEBlVut9ytDi/oV4gQ3oSFfk/P41ei463BGVuzpRZaJrXWAPwmMqlOdItK8RgeKXn8DL5PpndzYbebfewaiD4IqfpaEXpKFyxwOezpS8gGFmBh1gDS24KsnHm0/HWjeiE4hYxFP9z02l9HPRTAyWv3WtEp+k6LADHbB4e0/6SgB3GWg8rbSY6wG2oH3tamloz5U7P2yrkbFHMSqPLdmUMZLlCwPf+KQjZcRU5dFluBIFHhPX+aUCk7ZPBtQojhoFawEwJ56HqnBLwt1ckGt+n04m3Ij9+y0dRVOoqz0S1ajyeimJIVfcLx0ElNjJCEVLRCRgcHbR3e5bSJPGFItIQan/x3c7qVaMZOorcZ7fnpr1L4g1NK+tGtHTMwHWWDluXIlhk1lzNVIy4KEtkJiSjq8V3zBnBsPliXM1mP/NVWhMArY/9+S5l1TM6gV8ROqwcT99IqWsTSSeSB55hLyqKlxpWeN7rMvvJMIgaAv4ZIf8Rci8c6aqTaI07Vvi54zLVJ1JxoN1hYxFmI3kg1Coya+QPO8MfB7/ILX1TrdqDXGHXudrfuuitYT7CjA6wRggAmbtZtLT8B+Nbii94sDxoGPis4voWevUOYk1RVWY9STLHD+S7jBDXZy01oX6nYexkIJPQwxmexfVlx2crMfNI3BZ9eaejJIJAT2HNTOnDV9h3ODRTahyyWNFAjdhPrFhpQO4Q4RbEoWSdUr72BXCeV14f5etwNFntEOUcOgKc5rVk0meVjdrX6w3LI5nN5paUioNA/8/WtvAinQjHNw==
```



## 2. Partilha/Divisão de segredo (Secret Sharing/Splitting)



### Pergunta P2.1 - A





### Pergunta P2.1 - B







## 3. Authenticated Encryption



### Pergunta P3.1







## 4. Algoritmos e tamanhos de chaves



### Pergunta P4.1 - Itália (ECs "Banca d'Italia" e "Ministero della Difesa")









## Referências

[Wikipedia - */dev/random*](https://en.wikipedia.org/wiki//dev/random?fbclid=IwAR3oZenGHfHH0Zq5myM0nq90_IBgkuNUQ_VTqCoIl1K2BoY23Y3W0YrPiLw)

[stackoverflow - *differences between random and urandom*](https://stackoverflow.com/questions/23712581/differences-between-random-and-urandom)
