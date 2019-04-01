# Trabalho Prático 6 - 1/Abr/2019

## 1. Vulnerabilidade de codificação

### Pergunta 1.1 - 1

Considerando que o SO Linux 3.1, o Facebook e a Google tratam-se de *sofware* dito normal, em contraste com o *car software*, que recorre a métodos de desenvolvimento rigorosos. Assim, para o primeiro caso, considerar-se-ão 50 *bugs* por cada 1 000 SLOC, enquanto que para o segundo se considerarão 5.

Através do *website* [Information is beautiful](https://informationisbeautiful.net/visualizations/million-lines-of-code/), cada um dos *softwares* tem os seguintes valores de SLOC:
+ Linux 3.1 - 15 milhões SLOC
+ Facebook - 62 milhões SLOC
+ Car software - 100 milhões SLOC
+ Google - 2 mil milhões SLOC

Aplicando os valores definidos para o número de *bugs* por 1 000 SLOC, obtêm-se as seguintes estimativas:
+ Linux 3.1 - 750 mil *bugs*
+ Facebook - 3 100 mil *bugs*
+ Car software - 500 mil *bugs*
+ Google - 100 milhões *bugs*

### Pergunta 1.1 - 2

Não é possível estimar quantos dos *bugs* previamente calculados corresponderão a vulnerabilidades.

### Pergunta 1.2

Vulnerabilidades do projeto:
1. Limitação do acesso aos dados do utilizador associado e, no entanto, não implementar um mecanismo de cifra dos dados. A correção deste problema envolve a implementação de um mecanismo de cifra, ação que apresenta um nível de dificuldade reduzido.
2. Armazenamento de uma *password* em texto limpo para posterior autenticação do utilizador. Para resolver este problema, é recomendado o armazenamento de um valor de *hash* da *password* cuja derivação, recorrendo a bibliotecas criptográficas, é uma tarefa trivial.

Vulnerabilidades de codificação:
1. 