# Trabalho Prático 4 - 18/Mar/2019

## 1. RGPD (Regulamento Geral de Proteção de Dados)

### Pergunta P1.1

> O grupo escolheu analisar a secção *Storage limitation principle* do livro *Handbook on European data protection law*.

Tanto o artigo 5 (1) (e) do Regulamento Geral de Proteção de Dados (RGPD) como o artigo 5 (4) (e) da *Modernised Convention 108* requerem que dados pessoais sejam mantidos em formulários, que permitam a identificação de indivíduos, apenas pelo tempo necessário tendo em conta os propósitos para os quais os dados são processados. Assim, os dados devem ser apagados ou anonimizados quando esses propósitos forem cumpridos (**princípio da limitação de armazenamento**). Com este objetivo, o controlador deve estabelecer limites de tempo para remover ou efetuar uma revisão periódica, de forma a assegurar que os dados não são mantidos por mais tempo que o necessário.

Por exemplo, no caso *S. and Marper*, o Tribunal Europeu dos Direitos Humanos (*European Court of Human Rights* - ECtHR) estabeleceu que a retenção de impressões digitais e amostras de ADN por tempo indefinido era desproporcional e desnecessária numa sociedade democrática, considerando que os processos criminais contra os dois requerentes tinham sido terminados por absolvição e desistência, respetivamente.

O armazenamento de dados com objetivos de interesse público, científicos, históricos ou estatísticos pode ser efetuado por períodos de tempo superiores, desde que esses dados apenas sejam utilizados com os propósitos referidos. Devem ser implementadas medidas técnicas e organizacionais adequadas para o armazenamento corrente e utilização de dados pessoais, de forma a salvaguardar os direitos e liberdade dos indivíduos em questão.

A *Modernised Convention 108* também permite exceções ao princípio de limitação de armazenamento, na condição destas verificarem o seguinte:

* estarem previstas na lei;
* respeitarem a essência dos direitos fundamentais e liberdades;
* serem necessários e proporcionais para cumprir um número limitado de objetivos legítimos (e.g., proteger a segurança nacional, investigar e processar infrações penais, aplicar sanções penais, proteger o individuo em causa e os direitos e liberdades fundamentais de outros).

Assim, há necessidade de se prever a possibilidade de remover ou anonimizar dados pessoais que não são utilizados à algum tempo, manualmente ou automaticamente (periodicamente), quando se desenvolve *software*.


### Pergunta P1.2

Secção 3

1. Daniel
A pseudonimização permite a obfuscação da identidade de um indivíduo, bem como a tornar a sua procura impossível através de diferentes domínios de processamento de dados. Deste modo, considera-se que as técnicas de pseudonimização cumpram dois objetivos:
+ Os pseudónimos de um dado indivíduo não podem ser fáceis de identificar por partes exteriores a um dado contexto de dados.
+ Os pseudónimos não serão simples de reproduzir por terceiras partes de modo a evitar que diferentes contextos de dados utilizem o mesmo pseudónimo para cada determinado indivíduo, certificando que nenhum utilizador pode ser ligado a a qualquer pseudónimo.

Para tal, foram desenvolvido um conjunto de técnicas pela ENISA que pretendem proteger a identidade de indivíduos como medida de segurança.

2. Afonso
3. Mafalda

*Hashing with key or salt*

As funções de *hash* com chave podem ser utilizadas para anonimizar dados, uma vez que apagar a chave secreta elimina quaisquer associações entre pseudónimos e os identificadores iniciais. Isto seria equivalente, de forma geral, a gerar pseudónimos aleatórios, sem qualquer conexão aos identificadores iniciais.

Da mesma forma, poderiam utilizar-se funções de *hash* sem chave, mas que recebem um *salt* (dados aleatórios), pois se o *salt* é destruído de forma segura pelo controlador, não é trivial recuperar a associação entre pseudónimos e identificadores.

Contudo, o *salt* não possui as mesmas propriedades de imprevisibilidade que as chaves secretas (tamanho inferior) e as funções de *hash* com chave são consideradas, geralmente, criptograficamente mais fortes.

4. Daniel
*Cifra como técnica de pseudonimização*

A utilização de cifras simétricas, tais como o AES, é também considerado um método eficiente de gerar pseudónimos. Nestes, o identificador de um indivíduo é cifrado com uma chave simétrica que deverá ser conhecida apenas pelo controlador e processador dos dados, que, quando necessário, é utilizada para decifrar o pseudónimo para recuperar o identificador original.

Tal como no caso das funções de *hash*, as chaves utilizadas têm um tamanho mínimo de 256 bits, tamanho tal considerado razoável para segurança mesmo em contexto quântico. No entanto, ao contrário das funções de *hash*, o controlador pode, a qualquer momento, recuperar o identificador do sujeito através de uma operação de decifragem. Assim, torna-se mais difícil o *tracking* de indivíduos devido ao facto de que estes não necessitam guardar os identificadores.

[comment] Assim, a cifra simétrica pode ser utilizada em casos nos quais o controlador necessita seguir os sujeitos, bem como conhecer os seus identificadores.

Por outro lado, as cifras assimétricas têm também um conjunto de caraterísticas que as tornam úteis na pseudonimização. A sua estrutura de chave pública ajusta-se bem para contextos nos quais o controlador de dados responsável por efetuar a pseudonimização não está autorizado para recuperar a respetiva identidade, utilizando a chave pública correspondente a um controlador que guarda a respetiva chave privada. Deste modo, permite-se a separação de tarefas de segurança. Ainda mais, recorrendo à utilização de valores aleatórios assegura que os pseudónimos gerados não podem ser utilizados para seguir o percurso de um indivíduo através de vários contextos de dados.

É de notar, no entanto, que algoritmos de chaves assimétricas são bastante menos eficientes do que os de chaves simétricas, necessitando chaves de tamanho elevado, 3072 bits no caso do RSA. Mesmo recorrendo a curvas elíticas, que utilizam chaves bastante menores incorrem tempos de processamento mais longos.

5. Afonso
6. Mafalda

*Tokenisation*

Tokenização refere-se ao processo em que os identificadores de indivíduos são substituídos por valores gerados aleatoriamente, conhecidos como *tokens*, sem existir qualquer relação matemática com os identificadores originais. Assim, o conhecimento de um *token* é inútil para qualquer um, exceto o controlador ou o processador.

<!--
Como existe uma entidade que armazena o mapeamento oculto (de dados originais para um *token* aleatório), a re-identificação de indivíduos pelo controlador de dados será possível em todos os casos, incluindo para efetuar rastreamento, desde que haja apenas um mapeamento para cada identificador.
-->

Apesar da eficiência da tokenização, a sua implementação pode, dependendo do contexto, ser relativamente complexa. De facto, em várias aplicações pode ser necessário, por exemplo, a sincronização de *tokens* através de vários sistemas.

7. Daniel

### Pergunta P1.3 - 1

TODO: Afonso

### Pergunta P1.3 - 2

O projeto que se pretende desenvolver consiste numa aplicação educativa para crianças, que permite a disponibilização de informações e testes relacionados com temas de carácter pedagógico, bem como estatísticas de acerto para o utilizador corrente. Para além disso, esta aplicação deverá permitir que professores consultem o desenvolvimento dos seus alunos.

As credenciais de acesso dos alunos e professores são disponibilizadas pela escola. Cada aluno terá associado ao seu perfil os seguintes dados: número de aluno, nome completo, nome de utilizador, *password*, escola, ano e turma. Cada professor será caracterizado por: nome completo, nome de utilizador, *password* e escola.

Após a autenticação, os professores podem disponibilizar informações e testes para os alunos de um ano e turma, num determinado ano letivo. Posteriormente, os alunos realizam os testes, que são automaticamente corrigidos e acrescentados ao respetivo histórico. Este, por sua vez, apenas é acessível pelo próprio aluno. Mais ainda, o professor tem acesso às estatísticas dos testes por si efetuados, para os vários alunos.

A escola tem a possibilidade de apagar a conta de um aluno ou professor. Nesse caso, são removidos todos os dados associados ao utilizador, exceto o respetivo histórico (anonimizado), que é mantido para fins estatísticos.

Desta forma, a aplicação proposta respeita os critérios 3 (monitorização sistemática) e 7 (dados sobre sujeitos vulneráveis) enunciados na pergunta anterior, pelo que é necessário efetuar o DPIA.


### Pergunta P1.3 - 3


### Pergunta P1.4

