# Avisos

Essa arquivo markdown tem 3500 palavras, e aproximadamente 25000 caracteres, então é bem longo.

Óbviamente essa tabela não é definitiva, caso vocẽs queiram mudar ou excluir alguma coisa, fiquem a vontade, mas mudem o texto que eu escrevi relacionado a parte editada e adicionem o próprio de vocês, depois, deem commit no mesmo repositório baixado.

Esse arquivo tá longe de ser um documento formal, isso pode ser percebido ao longo das tabelas, onde minha sanidade mental foi se deterionando.

Tem algumas notas que eu escrevi sobre a implementação, mas aqui tem algumas importantes também, vou tentar ser o mais direto possivel nessa parte…

Pra esse banco de dados ser capaz de lançar um balancete mensal ele tem que incluir as entradas e saidas, infelizmente aqui só tem as entradas, isso significa que no “balancete” não vai entrar coisas como luz, agua, pagamento dos funcionários,  obras e etc. (falta de dados)

Um banco de dados tem uma interface diferente para cada tipo de público, isso significa que um usuário em especifico não deve ter acesso a coisas que não são visiveis pra ele

A principio, esse conceito não que violado aqui, mas os funcionários técnicamente deveriam fazer parte do banco de dados, já que eles estão envolvidos em muitas partes. 

-  Caso alguém marque uma limpeza, o dono teria que ir “pessoalmente” dizer para o funcionário fazer a limpeza, em tal hora e etc, já que o funcionário não conseguiria ver por conta própria.
- Ele também teria que fazer a alteração de todos os dados não visiveis para o usuário como: lançar infrações, e outros tipos de coisas.

Uma solução bem simples a primeira vista mas que tem um monte de problemas seria dar para os funcionários o mesmo tipo de acesso que o dono, violando assim o conceito, mas isso não é uma boa ideia… 
1. o funcionário conseguiria ver todo tipo de informação.
2. o funcionário conseguiria mexer em qualquer tipo de informação.

Provavelmente a fátima reclamaria disso tudo.

#### Baboseiras(não é necessário ler )

Recomendo lerem a esse arquivo no Obsidian com o tema Tokyo Night no modo claro

Hoje de manhã  um monte de crente me chamou para conversar sobre a biblia na graminha e lá tava com mó cheirão de maconha, então talvez eu estivesse chapado enquanto escrevia isso…?

Não vou dar commit ou mexer nisso aqui até o sabádo que vem

# Gerenciamento de Moradores e Responsáveis Financeiros
##  Pessoas

| Nome completo | Data de nascimento | Genero | CPF | RG  | Email | Numero de telefone |
| ------------- | ------------------ | ------ | --- | --- | ----- | ------------------ |
|               |                    |        |     |     |       |                    |

- [x] Será possível cadastrar novos moradores e editar dados de moradores existentes, além de consultar o histórico de todos os moradores que já foram cadastrados. 
- [x] O mesmo deve valer para o responsável financeiro do morador, se existir. 
- [x] O sistema deve armazenar nome completo, data de nascimento, gênero, CPF, RG, e-mail e número de celular, tanto do morador como do seu responsável financeiro, além de um contato de emergência do morador. 
- [x] O status do morador pode ser atual ou de ex-morador. Será atribuído um número identificador para cada morador.

### Mudança das tabelas de Pessoas e Responsáveis financeiros  

- Não há motivo para deixar os moradores e responsáveis financeiros em tabelas separadas, já que seria redundância 2 tabelas com os mesmos atributos, e ainda criaria o problema do que fazer caso o responsável financeiro e o morador fossem a mesma pessoa (nesse caso estou considerando que o morador estudante trabalhe e consiga morar lá)
- O atributo de estado de moradia é outra redundancia, o fato de um morador morar em algum quarto, por conceito, já é um tipo de relação, então não faz sentido colocar como atributo em uma tabela, além disso, isso criaria outro problema, o fato dos responsáveis financeiros não serem moradores, deixaria os valores nulos(redundância de acordo com a fátima), sem falar que é possivel calcular se o morador mora ou não em um lugar através de outras coisas, e também não resolveria o problema de histórico de moradores
- Não faz sentido ter um contato de emergencia como atributo, pois já que é exclusivo do morador, o responsável financeiro não deveria haver um e mesmo que houvesse, poderia ter a chance de haver repetição de valores caso o contato de emergencia do responsável financeiro e do morador fossem o mesmo, ou o contato de mergencia do morador fosse o próprio responsável financeiro, sendo assim vamos nos limitar a 1 contato de emergencia por moradia

>  O RG não vale de muita coisa se não tiver um estádo emissor,  mas como essa empresa nem é de verdade, então foda-se.

# Apartamentos
## Locais
| Nome        | Metragem total | Andar | Descrição | ID  |
| ----------- | -------------- | ----- | --------- | --- |
| Apartamento | 25m²           | 4     |           | 14  |
| Piscina     |                |       |           | 22  |
| lavanderia  |                |       |           | 33  |
## Studios (tipo de apartamento)

| Nome                              | Capacidade | Quartos | ID  | Banheiros |
| --------------------------------- | ---------- | ------- | --- | --------- |
| Studio individual                 | 1          | 1       | 1   | 1         |
| Studio compartilhado              | 2          | 1       | 2   | 1         |
| Apartamento de Três Quartos       | 3          | 3       | 3   | 1         |
| Apartamento de Três Quartos Suite | 3          | 3       | 4   | 3         |

## Apartamentos

| Número | Tipo (ID de studio) | possui varanda | é mobiliada | metragem por quarto | Onde fica (ID do local) |
| ------ | ------------------- | -------------- | ----------- | ------------------- | ----------------------- |
| 2      | 1                   | false          | false       | true                | 14                      |
### Cardinalidade

##### Studio para apartamento:
###### Minima: 
0, pois um tipo de apartamento não precisa de nenhum apartamento para existir,

###### Máxima:
N, pois um studio pode estar associado a vários tipos de apartamento
##### Apartamento para studio:
###### Miníma: 
1, pois um apartamento precisa ser algum tipo de studio para existir.

###### Máxima:
1, pois um apartamento só pode ser de um tipo de studio

##### Local para apartamento:
###### Minima: 
0, pois um local não precisa ser um apartamento necessariamente
###### Máxima:
1, pois um local pode ser apenas 1 apartamento
##### Apartamento para local:
###### Miníma: 
1, pois um apartamento precisa estar em algum local para existir.

###### Máxima:
1, pois um apartamento só pode estar em um local

## Moradia (local x Pessoa)

| Morador ID | ID local | Quarto(O quarto especifico em que o sujeito mora) | Data de Entrada | Data de saída | Contato de Emergencia |
| ---------- | -------- | ------------------------------------------------- | --------------- | ------------- | --------------------- |
| 24242      | 1        | 1                                                 | 6/2/2025        | 6/05/2025     |                       |

essa parte é bem dificil, mas essa provavelmente foi a solução com o minimo de redundancias…

Nesse modelo, a Moradia é uma relação entre pessoa e local, mas para essa relação ocorrer, é Obrigatório que tenha uma relação entre apartamento e moradia, então pode ser necessária a criação de um algoritmo para verificar se um local é um apartamento ou não

A data de saída nunca será nula, pois ela começa sendo a data de encerramento do contrato, mas caso o morador saia antes, basta atualizar.

#### Exemplos:
> Como faria pra verificar quantos metros tem o quarto que uma pessoa mora?

1. Primeiro basta ver se essa pessoa mora em algum lugar, primeiro acessamos a tabela pessoas, e verificamos se ela existe no banco de dados, existe? SUAVE, se não ela não tem nada haver com o condominio.
2. Segundo basta ver se ela aparece em algum lugar na Tabela Moradia, ela aparece? basta ver a data de entrada de saida, se elas estiverem dentro da data atual, então ela é um morador, se não, ou ela já saiu, ou ela nem morou em nenhum lugar
3. terceiro, de acordo com essa tabela, basta verificar o ID local, esse id nos levara para algum local da tabela Locais
4. Depois, basta acessarmos a tabela Apartamentos, e verificarmos as informações especificas sobre os quartos


> Como faria para verificar quais quartos estão disponiveis em um determinado apartamento?
1. Primeiro basta verificarmos na tabela apartamentos, o apartamento desejado, depois depois, verificamos o tipo de Apartamento, agora, 
2. Vamos a tabela studio, e colocamos o ID lá, por lá dá pra saber que tipo de apartamento é, iremos salvar as seguintes informações: Capacidade e número de quartos, vamos pegar como exemplo o Apartamento de 3 quartos (no algoritmo provavelmente vai ser criado uma matriz, onde a quantidade de colunas vai ser o numero de quartos e o numero de linhas será o numero de vagas por quarto, poderia ser o contrário também)
3. Depois, voltamos para a tabela apartamentos,e anotamos o valor do ID do local
4. Vamos para a tabela moradias, e filtramos todos os registros com esse ID especifico, depois filtramos de acordo com a data, para mostrar apenas as moradias ativas
5. Depois nossa matriz vai estar provavelmente assim: {} {} {}, agora é só colocar um x para cada registro que aparece em cada quarto
6. Morador mora no quarto 1,:  {x}{}{}
7. Morador mora no quarto 3: {x}{}{x}
8. caso esses sejam os unicos registros, isso significa que o quarto do meio desse apartamento está livre.
### Cardinalidades

##### Apartamento para pessoa:
###### Minima: 
0, pois um tipo de apartamento pode estar vázio.
###### Máxima:
N, pois podem haver várias pessoas em um apartamento, tanto no passado, tanto no presente
##### Pessoa x Apartamento:
###### Minima: 
0, pois uma pessoa pode ser simplesmente um responsável financeiro,

###### Máxima:
N, pois uma pessoa pode mudar de apartamento e como isso é um HISTÓRICO, naturalmente algumas pessoas podem ter morado em diversos apartamentos
# Contratos de alugueis (apartamento x pessoa)

| ID responsavel | ID apartamento | Quarto | Valor Base aluguel | Data de contratação | ID contrato | Valor base condominio | Data finalização |
| -------------- | -------------- | ------ | ------------------ | ------------------- | ----------- | --------------------- | ---------------- |
| 2582309        | 29428948       | 1      | 1400               | 12/12/2025          | 54256       | 200                   | 12/12/2025       |

- é inutil armazenar documentos fisicos ou digitais em um banco de dados, e como é OBRIGATÓRIO por lei a emissão de um documento físico ou digital(em forma de pdf), não faz sentido armazenar todos os dados de um contrato em um banco de dados, sendo que todos eles são disponíveis em um documento, sendo assim, nessa implementação, o intuito é a automatização e a facilitação de consulta de certos tipos de dados(os mais usados de preferência).
- Detalhe que os valores são apenas bases, então, eles não serão alterados (pelo menos não aqui).



#### Requisitos

- [x] Um novo morador deve ser relacionado a um apartamento cadastrado. 
- [x] O apartamento pode ser habitado por mais de um morador, se for do tipo Studio Compartilhado ou Apartamento de Três Quartos Normal/Suíte. Porém, em ambos casos de Apartamento de Três Quartos, cada quarto deve ser habitado por apenas um morador. 
- [x] Deve ser possível cadastrar os dados que explicitem qual o tipo de apartamento (e quarto, se for o caso) que o cliente está estabelecendo o contrato, bem como o valor do aluguel mensal acordado, os holerites ou extratos bancário do responsável financeiro, a data de entrada do morador e a data de fim de contrato. 
    Vale lembrar que esse modelo não especifica nada sobre entrada de saída dos moradores, oque é um erro grave, pois nem sempre um morador começa a morar em um lugar exatamente no começo do contrato, e nem sempre saí exatamente no fim do contrato. 
     >> **Alerta de Baboseira**
     >> É possivel pensar nas datas de inicio e fim de um contrato, como o limite inferior e superior, respectivamente da data de inicio de fim moradia,

- [x] Para renovação de contrato, deverá ser possível editar as datas. Deve ser possível ter acesso ao histórico de contratos já cadastrados no sistema. 
- [x] Será atribuído um número identificador para cada contrato.

### Cardinalidades

##### Apartamento para pessoa:
###### Minima: 
0, pois um tipo de apartamento pode estar vázio.
###### Máxima:
N, pois um apartamento pode ter tido diversos responsáveis financeiros.

##### Pessoa para Apartamento:
###### Minima: 
0, pois uma pessoa pode ser simplesmente um morador.

###### Máxima:
N, pois uma pessoa pode ter diversos contratos, caso ela tenha se mudado, ou caso ela seja uma mãe e tenha assinado um contrato para seus dois filhos diferentes.

#  Multas

Aqui, decidi separar o sistema de multas em regras e infrações, sendo “Regras”, uma tabela de regras a serem seguidas, e infrações a ocorrência da quebra dessas regras, as infrações podem ser divididas em avisos, e em multas, no caso, cada regra tem um valor tolerância, que é quantidade de vezes que um aviso pode ser lançado antes de ser aplicado a multa.
## Regras

| Tipo | Valor      | Tipo de infração | Tolerância | Descrição |
| ---- | ---------- | ---------------- | ---------- | --------- |
| 1    | Valor base | Vandalismo       | 3          |           |
## Infrações

| ID multa | ID Responsável | Tipo de regra | Data de infração | Detalhamento | Tipo  |
| -------- | -------------- | ------------- | ---------------- | ------------ | ----- |
| 1        | 24828          | 1             | 25/02/2025       | …            | Aviso |
|          |                |               |                  |              | Multa |

- [x] Os possíveis tipos de infrações que podem ser cometidas por moradores devem ser cadastradas e consultadas quando necessário, atrelado ao valor da multa cobrada a um morador que cometer a infração. 
- [x] O sistema deve permitir o cadastro e consultas de infrações cometidas, com responsabilização do morador, tipo de infração cometida, data da infração e detalhamento. 

### Cardinalidades

##### Regra para infração:
###### Minima: 
0, pois uma regra não precisa ter nenhuma infração.
###### Máxima:
N, as regras podem ter sido quebradas diversas vezes.

##### Infração para Regra:
###### Minima: 
1, essencialmente, uma infração significa a quebra de uma regra, então não faz sentido ser uma infração se não tiver nenhuma regra pra ser quebrada.

###### Máxima:
1, uma infração, pelo menos nesse modelo, diz respeito a apenas 1 regra.
 
##### Infrações para pessoa:
###### Minima: 
1, se uma regra foi violada, foi porque alguém violou.
###### Máxima:
1, uma regra pode ser violada apenas por 1 pessoa(pelo menos nesse modelo).

##### Pessoa para Infrações:
###### Minima: 
0, uma pessoa não precisa violar nenhuma regra.

###### Máxima:
N, uma pessoa pode ter violado diversas regras.



# Gerenciamento dos Serviços Oferecidos ao Morador
## Areas comuns

| ID local | Nome       | Alugavel | Preço por hora |
| -------- | ---------- | -------- | -------------- |
| 223      | Piscina    | True     | 50             |
| 252      | Academia   | False    | 0              |
| 257      | Lavanderia | False    |                |
### Cardinalidades

##### Local para Area Comum:
###### Minima: 
0, pois um local não é necessáriamente uma area comum.
###### Máxima:
1, um local pode ser apenas 1 area comum, isso é não pode haver 2 areas comuns ou 1 apartamento e 1 area comum com o mesmo local.
##### Area Comum para Local:
###### Minima: 
1, pois uma Area comum precisa de um local.
###### Máxima:
1, pois uma area comum pode ter apenas 1 local 

---
# Reservas

| ID usuário | ID area | data de locação | data  | quantidade de horas |
| ---------- | ------- | --------------- | ----- | ------------------- |
| 16574512   | 24294   | 12/1/2021       | 12/02 | 7                   |
### Cardinalidades

##### Area para pessoa:
###### Minima: 
0, uma area pode não ser utilizada por ninguém.
###### Máxima:
N, pois uma area pode ter sido alugada por diversas pessoas ao longo do tempo( essa tabela é um histórico de reservas).

##### Pessoa para Area: 
###### Minima: 
0, uma pessoa pode não alugar nenhuma area.

###### Máxima:
N, uma pessoa pode ter alugado diversas areas.
## Serviços
| Tipo                  | ID do TIPO de serviço | Preço base |
| --------------------- | --------------------- | ---------- |
| Limpeza pós reserva   | 1                     | 100        |
| Limpeza apartamento   | 2                     | 200        |
| Limpeza Piscina       | 3                     | 300        |
| Limpeza de manutenção | 4                     | 40         |
| Limpeza quarto        | 5                     | 50         |
## Pedidos de serviços

| ID do tipo de serviço | data pedido | Hora  | ID cliente | ID area | Status    | Data para o serviço ser feito | Horário para o pedido ser realizado | ID do pedido | Descrição |
| --------------------- | ----------- | ----- | ---------- | ------- | --------- | ----------------------------- | ----------------------------------- | ------------ | --------- |
| 5                     | 17/02/2025  | 16:00 | 125902     | 24525   | Agendado  | 12/11                         | 14:00                               | 0011         | quarto 2  |
| 2                     | 15/02/2025  | 15:00 | 123424     | 5252    | Concluido |                               |                                     |              |           |
| 3                     |             |       |            |         |           |                               |                                     |              |           |
| 4                     |             |       |            |         |           |                               |                                     |              |           |
### Cardinalidades

##### Serviços para pedidos:
###### Minima: 
0, pois um serviço não precisa ser feito por ninguém pra existir.
###### Máxima:
N, um serviço pode ser feito várias vezes.
##### Pedidos para Serviços:
###### Minima: 
1, um pedido tem que ter algum serviço.
###### Máxima:
1, um pedido trata apenas de 1 serviço.

- [ ] Alguns serviços pay-per-use são oferecidos ao morador, ou seja, serviços que não estão incluídos no valor do aluguel mensal, como uso da ~~lavanderia comum, limpeza de apartamentos e quartos individuais~~, aluguel de vaga de estacionamento, aluguel de bicicletas e reservas de áreas de lazer do prédio. 
- [x] Será possível cadastrar todos os pedidos de serviços feitos por moradores, bem como ter acesso ao histórico de pedidos, com dados sobre o morador que solicitou o serviço, a data da solicitação, o(s) dia(s) em que a solicitação será cumprida, e o valor cobrado pelo serviço.

 Detalhe, aluguel de vaga de estacionamento e aluguel de bicletas são conceitos que são muitos mais complexos do que um serviço qualquer como uma limpeza, já que requer o gerenciamento dessas pŕoprias entidades, e consequentemente, uma tabela pŕopria para cada tipo de vaga ou bicicleta e etc, então não irei tratar desses casos aqui, mas caso vocês queiram, talvez o modelo acima possa ser usado.

--- 

# Gerenciamento de Cobranças e Inadimplências 


- [x] É necessário a emissão de cobranças mensais a um morador que possui um contrato ativo. 
- [x] O valor da cobrança terá como base o valor acordado como aluguel mensal, porém esse valor poderá sofrer incrementos caso o morador tenha cometido infrações e/ou usufruído de algum serviço pay-per-use. 
- [x] As cobranças de cada morador são registradas por mês, e existe uma data limite de pagamento da cobrança total. 
- [x] No sistema, a cobrança deve possuir três estados possíveis: a pagar, paga ou vencida. Caso ela não seja paga após a data limite, ela será uma cobrança vencida, que deverá ter seu valor total atualizado com o acréscimo de uma multa e juros.

## Tabela de cobranças

| ID pessoa | Valor | data limite | Estado  |
| --------- | ----- | ----------- | ------- |
| 2424      | 3400  | 21/01       | a pagar |
| 25252     | 2420  |             | ativa   |
| 24242     | 343   |             | a pagar |
|           |       |             |         |
- A maneira como a cobrança é organizada é um GRANDE problema, vamos aos pontos
   1.  O conceito de existir um atributo de estado poderia até funcionar, mas isso gera a seguinte redundancia: Dá pra saber se uma cobrança está vencida ou não através do prazo de pagamento, então ter um estado pra isso é, essencialmente, inútil(de acordo com a fátima).
   2. O que acontece seja necessário saber qual era o valor da cobrança em um mês especifico, mas o filho de uma puta atrasou o pagamento por 5 meses e os juros foi pra casa do caralho? 
   3. Nesse modelo em que todos os serviços estão todos juntos, os juros tecnicamente se aplicariam para todos os serviços. 
   4. Caso ocorra um pagamento parcial, o que deveria ser pago? o aluguel ou a limpeza?, não pagar o aluguel tem consequencias totalmente diferentes de não pagar uma limpeza.
   5. O quarto é que, a maneira como está organizada está tudo junto, dessa forma, ficaria bem dificil filtrar do que CARALHOS essa cobrança está tratando, tanto para pessoa  que está pagando, tanto para o dono que quer saber de qual fonte vem o dinheiro, se é das pessoas usando a piscina, se é dos alugueis ou etc.
## Tabela de cobranças

| ID cobrança | Descrição              | Valor | Direção | ID origem | Tipo de origem    | data  |
| ----------- | ---------------------- | ----- | ------- | --------- | ----------------- | ----- |
| 00001       | ** baboseira aleatória | 3400  | Entrada | 1234529   | Multa             | 21/01 |
|             | ** baboseira aleatória | 242   | Entrada | 22909     | Aluguel           |       |
|             | ** baboseira aleatória | 343   | Entrada |           | Pedido de serviço |       |
|             |                        |       | saida   |           | Conta de energia  |       |
Essa provavelmente deve ser a parte mais complicada de explicar, então… eu vou explicar atributo por atributo…


- A ID de cobrança é uma chave unica exclusiva de cobrança
- A descrição é qualquer merda aleatória, eu nem sei se deveria ser incluida mas enfim.
- O valor é o valor da cobrança obviamente
- A direção é a parte mais importante, está dizendo se é uma entrada ou uma saída, uma entrada significa que o Dono está dinheiro, e a saída significa que o dono está perdendo dinheiro, naturalmente, como a modelagem antiga possuia os funcionários, essa parte era mais necessária, mas como é um caralho de um balancete e se trata de saídas e entradas, entao decidi manter, até porque, o dono também gasta com muitas coisas, como: agua, impostos e etc.
- a ID da origem é de onde vem essa cobrança, nesse caso, ela ficaria meio genérica, pois poderia vir de qualquer lugar, no entanto…
- do lado dela tem um campo chamado Tipo de origem, que especifica de onde ela vem, então, é como uma especie de chave composta
- ID pessoa, nesse caso, vai ser pessoa que vai pagar
- Data seria a data de emissão da cobrança.
### Cardinalidades

Não é possivel calcular pois é um conceito que meio que foge de banco de dados, irei perguntar pra fátima se é aceitavel ter essa chave dupla.
# Gerenciamento Financeiro

- [x] Será possível que o administrador tenha acesso ao histórico de movimentações do caixa. Para isso, deverá ser possível cadastrar todos os tipos de receitas e despesas relacionadas à operação predial, classificadas por categorias gerais e subcategorias: as receitas oriundas das cobranças, e as despesas oriundas do pagamento de serviços terceirizados contratados pelo prédio, de contas fixas, itens comprados e outros custos adicionais, que deve ter suas características detalhadas com subdivisões. 
- [x] A partir disso, é possível cadastrar e classificar as movimentações financeiras, onde cada movimentação deve estar associada à uma categoria e deve possuir informações de data e valor total. 
- [ ] As movimentações de entradas e saídas devem estar ligadas a uma conta.
   - (Pressumindo que se trate de uma conta bancária) Tecnicamente isso é problema do banco, e não faz sentido armazenar informações desse tipo em um banco de dados de um condominio, provavelmente, seria necessário o uso de uma aplicação externa
- [x] Tais cadastros possibilitarão a geração de relatórios no formato de balancete, um documento que apresenta um resumo das receitas e despesas do condomínio em um período mensal, com o registro do saldo mensal (diferença entre arrecadação e as despesas do mês)
## Pagamentos

| ID cobrança | Data de pagamento | Valor | Forma de pagamento     | data  |
| ----------- | ----------------- | ----- | ---------------------- | ----- |
| 15353       | 22/02             | 3400  | Pix                    | 21/01 |
| 15556       | 20/02             | 242   | Transferencia Bancária |       |

### Cardinalidades

##### Pagamentos para cobranças:
###### Minima: 
1, só dá pra pagar o que é cobrado(pelo menos nesse modelo).
###### Máxima:
1, um pagamento só pode pagar 1 cobrança em especifico.
##### Cobranças para serviços:
###### Minima: 
0, Não é necessário pagar.
###### Máxima:
N, Naturalmente, como existe o conceito de pagamentos parciais, uma pessoa pode pagar uma cobrança diversas vezes.

---
# Tabelas não utilizadas que salvei apenas por foda-se
## Contratos trabalhistas (função x pessoa)

| ID pessoa | id funcao | Remuneração base | horas diarias | vale alimentação | vale refeição | vale transporte | Prox ferias |
| --------- | --------- | ---------------- | ------------- | ---------------- | ------------- | --------------- | ----------- |
| 16574512  | 239239    | 2000             | 15            |                  |               |                 |             |
## Instancia de trabalho(contrato trabalhista x instancia de entrada e saída)

| ID Contrato | Horas minimas | horas trabalhadas | Status     | data      | ID log |
| ----------- | ------------- | ----------------- | ---------- | --------- | ------ |
| 15353       | 12            | 6                 | Incompleto | 1/02/2025 |        |
## instancia de holerite (instancia de trabalho x contrato x serviços)

| ID Contrato | remuneração | vale refeição | vale transporte | vale alimentação | bonus | INSS |
| ----------- | ----------- | ------------- | --------------- | ---------------- | ----- | ---- |
| 15353       | 5000        | 200           | 200             | 2400             | 500   | 120  |
