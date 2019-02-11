# Trabalho Prático 0 - 11/Fev/2019



## 1. Números aleatórios/pseudoaleatórios



### Pergunta P1.1

Para responder a esta questão, torna-se necessário evidenciar a diferença entre a geração de números pseudoaleatórios com o recurso ao dispositivo `/dev/random` e ao `/dev/urandom`. Estes são ficheiros especiais do sistema *Linux* que têm acesso ao ruído presente no sistema operativo, utilizando a entropia disponível para gerar *bytes* ou uma *seed* pseudoaleatórios, respetivamente.

Por um lado, o dispositivo`/dev/random` requer espera pelo resultado (*bytes*), pois utiliza uma *entropy pool*. Assim, este mecanismo pode bloquear enquanto não for possível gerar dados aleatórios suficientes. Contudo, a realização de chamadas ao sistema operativo permite aumentar a entropia disponível e, consequentemente, possibilitar a geração da quantidade de *bytes* solicitada. Faz sentido utilizar este dispositivo quando são exigidos maiores níveis de qualidade na aleatoriedade.

Por outro lado, o dispositivo `/dev/urandom` não bloqueia devido à reduzida entropia disponível. Este mecanismo retorna valores independentemente de existir entropia suficiente, caso em que, teoricamente, estes são vulneráveis a ataques criptográficos. Neste caso, a entropia apenas é utilizada para gerar uma *seed*, com qualidade superior ou inferior (de acordo com a entropia disponível). De seguida, os *bytes* do resultado são gerados por um *pseudorandom number generator*.

O comando apresentado no início (`openssl rand -base64 1024`) permite gerar *bytes* *pseudoaleatórios*, após fornecer, uma vez, uma *seed* ao  *pseudorandom number generator*, e a codifica os mesmos em *base64*. É utilizado o ficheiro `$HOME/.rnd` ou `.rnd` (poderiam ainda considerar-se ficheiros adicionais passados como argumento) para gerar a *seed*.

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
