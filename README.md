# MOIP API TESTE

Esse teste se destina aos endpoints pertencentes a API do Moip, sendo reduzidos em escopo para Clientes, Pedidos e Pagamentos.

## Introdução

Para a execução dos cenários, foram realizados testes manuais, porém, os arquivos estão próximos de serem alterados e se tornarem automatizados.
A solução para a realização, se deu com o uso do **Postman**, um software gratuito, que pode ser usado para desenvolvimento de API e outras características. O Postman é um software teoricamente simples, mas bastante poderoso, e é por meio dele que poderemos checar os testes.

### Pré requisitos

Para a execução do ambiente, é necessário que você possua instalado o Postman em sua máquina, como também possua uma conta na plataforma de Sandbox da Moip.

```
Download Postman via https://www.getpostman.com/apps
```

```
Crie sua conta sandbox moip via https://conta-sandbox.moip.com.br/
```

## Iniciando

### Criando Clientes

Uma vez que foram executados os passos acima, é necessário que seja compreendido o funcionamento da **API**. Com isso, é disponibilizado uma referência para melhor entendimento.

```
https://dev.moip.com.br/v2.0/reference
```
Visto isso, faça com que o entedimento de Autenticação esteja claro, pois é necessário o uso de Token e Chaves para conexão com a API. 
Assumindo o passo anterior, ao abrirmos o Postman, teremos o seguinte ambiente:

![img1](https://user-images.githubusercontent.com/8397519/40283585-e9a01244-5c56-11e8-9191-f97b640fdd7d.png)


### Overview

Nas seguintes imagens, comentarei simples campos que são preenchidos com maiores detalhes.

```
Body
```
![img2](https://user-images.githubusercontent.com/8397519/40283656-11299122-5c58-11e8-9ac9-27772954fe5d.png)

No campo acima, estou criando um usuário simples, baseado nas mandatórias da **API** para testes. Isso inclui responses envolvendo **JSON** e códigos **HTTP**.

*O Content-Type está setado como também o application/json para execução.*

```
Pre-request Script
```
![img3](https://user-images.githubusercontent.com/8397519/40283706-eda64906-5c58-11e8-96a7-072f3bdce0c8.png)

**Pre-request Script** são pedaços de códigos que são executados antes de um request. 
Como será utilizado uma feature do Postman, o **Postman BDD**, é necessário definir o código acima para execução. 

**Postman Behavior Driven Development (Postman BDD)**

O **Postman** **BDD** permite usar a sintaxe do **BDD** para estruturar os testes e usar a sintaxe que é executada no postman de maneira que não atrapalhe a execução para os assertions. Abaixo é listado um simples comando de como funciona:

```
it(‘should be a 200 response’, () => {
 response.should.have.status(200);                           
});
```
Checa se o response executado por um método **GET, POST, DELETE**, etc é a mensagem **HTTP 200**, Successful Request. 

**Aplicando o BDD**

Para o evitar de problemas, abra uma nova aba no postman, dentro de sua collection e execute o método **GET** no link a seguir:
```
http://bigstickcarpet.com/postman-bdd/dist/postman-bdd.min.js
```
Logo após, vá até a aba de **Tests** ao lado de **Pre-request Script** e execute o seguinte comando:

```
postman.setGlobalVariable('postmanBDD', responseBody);
```
**Resultado**
![img4](https://user-images.githubusercontent.com/8397519/40283956-424f2f5a-5c5d-11e8-9a9f-d9498826f165.png)

Com isso, podemos seguir normalmente com os testes.


**Aba de testes**

Nessa aba, estão listados testes para checagem do nosso método que será criado por meio da **URL** concebida pela **API**.

![img5](https://user-images.githubusercontent.com/8397519/40284091-cf418b2c-5c5f-11e8-9490-8518acff0c59.png)

Podemos enxergar na primeira linha, uma chamada do **BDD**, basta apenas copiar e colar no topo do arquivo.

*Exemplo 1*

```
describe("Checagem básica se está no formato JSON", ()=>{
    it('Deve retornar uma resposta válida com HTTP 201', () => {
    response.should.have.status(201);
    response.should.be.json;
    response.body.should.not.be.empty;
  });
});
```
O exemplo que vemos acima é listado nas primeiras linhas do arquivo de teste e ele nada mais é do que uma simples checagem. Como estamos trabalhando com o formato **JSON**, é importante checar se o que é retornado é o que é esperado como response.

Ao criarmos um usuário, o output esperado é de um código **201 HTTP** via método **POST**. Por isso esperamos por um código **201**. Esse padrão é observado para o padrão **BDD**.

*Exemplo 2*

Em seguida vemos outro describe, que é usado o pm. O pm vem de postman e ele nos permite fazer checagens por meio dos ***tests***, simplificando a vida e o trabalho para grandes quantidades de testes.

```
   pm.test("Contém o id proprietário no body da mensagem", function () {
    pm.expect(pm.response.text()).to.include("ownId");
   });
   
```
Para criação de um usuário na API, existem os campos de **require** que são mostrados na referência. Para visualização, listei apenas dois, que pode representar bem o que é um usuário dentro do cenário.

```
ownId
```

É uma string com características **REQUIRED** que é o Id próprio do cliente. 
Referência externa. Limite de caracteres: (65).

```
fullname
```

É também uma string com característica **REQUIRED** sendo o nome completo do cliente.
Limite de caracteres: (90).

*Exemplo 3*

```
"it('should be an error response', () => {",
 "    response.ok.should.be.false;            // 2XX",
 "    response.error.should.be.true;          // 4XX or 5XX",
 "    response.clientError.should.be.true;    // 4XX",
 "    response.serverError.should.be.false;   // 5XX",
 "    response.should.have.status(404);",
"    response.statusType.should.equal(4);",
"});",

```
O exemplo acima, nos mostra de maneira clara o que é que deve retornar como error tanto por parte do usuário quanto por parte do server. Com os responses dados, podemos checar por errors 4xx gerados pelo lado do cliente, 5xx gerados pelo lado do servidor e 2xx para resultados executados por meio dos métodos, ex 201 e 200.
*Se houver dúvidas, para cada descrição de teste, existe uma explicação e está dividida pra evitar dificuldades de leitura"

**Execução**

Por fim, chegamos na criação do nosso usuário. Feito isso, podemos ver os resultados de maneira rápida sem necessidade de explicação de cada detalhe, podendo ser visível o que é gerado por meio dos comandos.

Abaixo, faço a execução do método **POST** para criação do cliente na **API**. É esperado um **201 HTTP CODE** 


![img7](https://user-images.githubusercontent.com/8397519/40284510-98746dc4-5c66-11e8-9a8a-5cead6c3b7e4.png)

**Checando o Passed**

Logo abaixo do botão **SEND** vemos o status da nossa requisição, sendo demonstradao como ``` Status 201 Created ```.
É nos mostrado que o nosso cliente foi cadastrado com sucesso e **4** testes passaram na nossa bateria. Vemos que são **4/10** o que pode soar estranho para algunns. Porém, os testes que passam, são exatamente os que importam para nós.

``PASS`` "Contém o id proprietário no body da mensagem". Vemos que o ownId fora preenchido.  

```PASS``` "Contém o nome no body da mensagem". Representado pelo nosso fullname. Mais uma vez, é o que esperávamos.  

```PASS``` Checagem do formato JSON, vemos que funcionou.  

``PASS`` Além do 201, vemos que um OK também é enviado. 

**Checando o Failed**

![img8](https://user-images.githubusercontent.com/8397519/40284682-3d4cf81e-5c69-11e8-97cd-8205435736f7.png)

``` FAIL 2 ``` Nesse *Fail* vemos que ele retorna quando encontra algum, como não aconteceu, ele foi reprovado no teste.

``` FAIL 3 ``` Como o nosso *201 HTTP CODE* retornou, o teste de não passar, foi reprovado. 

``` FAIL 5 ``` Também não houve error por parte do servidor na nossa requisição, esse teste foi reprovado.

``` FAIL 6 ``` Não houve por parte do cliente um erro notório, não aconteceu um 4xx, logo, foi reprovado.

``` FAIL 7 ``` Praticamente um sinônimo do *FAIL 5*

``` FAIL 8 ``` Equivalente ao FAIL 6, não retornando error 4xx, 404.

**Observação**

Até agora, toda a bateria de testes se deu por meio de um cenário ideal, onde tudo é controlado e tentado a replicar o que acontece de fato no dia a dia para que todos os passos sejam executados corretamente. Porém, o próximo passo, iremos checar o seguinte cenário.

1. Será repetido o mesmo método, sem nenhuma alteração. 
2. Será obtido resultados diferentes com características parecidas ao do exemplo acima.
3. Propor uma possível melhoria para melhor resposta para o usuário no campo de Status do **HTTP CODE**.

**Executando o método GET novamente**

O método GET fora executado novamente com os mesmos parâmetros. O ato de repetir a cena, se dá porque no dia a dia, é entendível que quem usa a API do Moip, lida com vários tipos de clientes com vários tipos de conexões. 
Ex: "Está sendo criado um cliente e por algum erro na conexão ou falha na mesma, o usuário clica mais de uma vez em um botão de cadastro, sendo que o formulário já foi enviado, e pode ser retornado um *400 BAad Request* para o usuário sem demais especificações."
O que pode ser explorado, aqui, seria a aplicação de renderização de página para usuário, o mostrando que ele é o responsável pela causa da falha e listando por meio de checagem de campos preenchidos, dizer ao mesmo qual campo ele preencheu ou não preencheu para gerar o erro.

Abaixo segue só para fins de observação o output do cenário citado acima.

![img10](https://user-images.githubusercontent.com/8397519/40285090-5e0e9a80-5c6e-11e8-89e7-a5a03af31681.png)

Vemos que o body é diferente do output de quando o usuário é criado. É claro para o tester and desenvolvedor o erro, mas isso não demonstrado muita das vezes de maneira clara para o usuário qual o fator gerador do error.

![img9](https://user-images.githubusercontent.com/8397519/40285200-efc5fcc4-5c6f-11e8-8f17-f06c23e69db7.png)

Os dois primeiros *FAILS* são claros de ser entendidos, pois não vindo o campo esperado como também a maneira que o format JSON é gerado, gera dúvida para o response do javascript.

Vemos que o envio do OK falhou, logo o teste foi reprovado, mostrando o que esperávamos. 

E por fim, o error 4xx não consegue ser retornado ao usuário. Porém, só basta acrescentar um 400 entre []. Mas para fim de aprendizado, ele pode ser mostrado como exemplo. 

E vemos os **"Passeds"** batendo justamente com o que é esperado para os testes escritos no programa.

![img11](https://user-images.githubusercontent.com/8397519/40285146-42c45c50-5c6f-11e8-836a-e1669599bdb0.png)

```PASS``` Vemos que o pass para Id é estranho, pois ele checa a string retornada para um identificador de "path"

```PASS 2``` Retornou um erro, o teste foi ativado

```PASS 3 ``` O HTTP 201 code não retornou. Correto

```PASS 4 ``` Retornou um erro por parte do cliente. Nesse campo, se visto em comparação com o último FAIL, é dado pelo simples erro de sintaxe, mas que pode passar sem ser visto por alguns QA do time. Porém ele retorna como esperado. Correto.

### Adicionando Cartão de Crédito 

Mais uma vez, criaremos uma nova aba ou arquivo como prefira falar para dentro da nossa **collection**, da qual, estão separadas como arquivos: *Moip-cliente, Moip-pedidos, Moip-pgamentos*. 

Para adicionar o cartão, a Moip disponibiliza um modelo claro e limpo de como adicionar a um determinado cliente. 
Abaixo, eu listo qual o campo que uso e qual o arquivo JSON é injetado. 

***Note que para fazer outra requisição, usaremos ainda o método POST com uma URL diferente, onde será passado o id do cliente, gerado quando criamos nosso cliente.***

É disponibilizado também, alguns modelos de cartão para o cadastramento. Tudo isso na referência da API. Tendo isso em vista, passamos o ID do cliente, no meu caso, o criado anteriormente, foi ```CUS-TQO0IQ4TWU67```. Nos próximos passos, veremos como isso será possível de obter.

![img12](https://user-images.githubusercontent.com/8397519/40285414-0adf7be6-5c72-11e8-8c8b-672cfa309719.png)

O input JSON aplicado é identico ao oferecido pelo exemplo da API. Lembrando, utilize os cartões fictícios fornecidos pela referência. Tentei utilizar outros, obtive erros. 

Chegamos assim, na parte de testes, que é o que importa para minha avaliação :) 

*Os arquivos estão nas collections, se ficar difícil de visualizar, vá até cada uma delas e abra no Postman*

![img13](https://user-images.githubusercontent.com/8397519/40285479-d5f58136-5c72-11e8-8fff-80dbae05af99.png)

Os testes são bastante parecidos para o de criar, porém, tem o checar de alguns campos a mais, que são gerados pelo response do nosso metódo.  ***Lembrar de add o ID do usuário na URL do POST method***

```
//Nome do usuário
  pm.test("Contém o o método da compra do cartão", function () {
    pm.expect(pm.response.text()).to.include("method");
  });
  //Id do usuário
  pm.test("Contém a bandeira do cartão", function () {
    pm.expect(pm.response.text()).to.include("brand");
  });
  // Primeiros 6 dígitos do cartão
  pm.test("Contém os 6 primeiros números do cartão", function () {
    pm.expect(pm.response.text()).to.include("first6");
  });
  // Últimos 4 dígitos do cartão
  pm.test("Contém os 4 ultimos números do cartão", function () {
    pm.expect(pm.response.text()).to.include("last4");
  });
  
 ```
É adicionado dois testes e mudado os dois primeiros. Faço a checagem do método, que para o exemplo atual, retornará como **CREDIT_CARD**, como também a marca e os números referentes. Explorei somente esses campos, pois para os demais, basta executar *ctrl+c e ctrl+v*, como o intuito é demonstrar o conhecimento, me limitei a eles.


**Executando**

Quando executo o método **post** o seguinte output para o **body** é retornado.

![img14](https://user-images.githubusercontent.com/8397519/40285563-f5a79e8c-5c73-11e8-95da-5db10a22328c.png)

Em seguida, os testes que passaram, temos:

**Checando o Passed**

![img15](https://user-images.githubusercontent.com/8397519/40285575-1ebfe626-5c74-11e8-9318-7d08bca2ce6e.png)

Vemos o *Status: 201 Created* mostrando que a criação do cartão para o cliente funcionou. Logo, podemos checar os "PASSes"

```PASS``` Contém o o método da compra do cartão - Esperamos isso, OK.

```PASS``` Contém a bandeira do cartão - Esperamos isso, OK.

```PASS``` Contém os 6 primeiros números do cartão - Esperamos isso, OK.

```PASS``` Contém os 4 ultimos números do cartão - Esperamos isso, OK.

```PASS 1``` Recebe informações do cliente - Deve retornar uma resposta válida - Vemos que fora retornada, OK.

```PASS 3``` Recebe informações do cliente - Envia um OK como resposta - Foi enviado o status, OK.

Vemos por meio de testes simples, o que é retornado. Como solução ou melhoramento para os testes que passarão, não tenho nenhuma em mente, acredito que elas satisfazem o que é necessário para uma boa navegação do cliente.

**Checando o Failed**

![img16](https://user-images.githubusercontent.com/8397519/40285616-dfbfab36-5c74-11e8-9b8e-39d4ef798153.png)

``` FAIL 2``` Vemos que o 201 retorna para o usuário, logo o não retornar dele, garante que o teste falhe - OK.

``` FAIL 4``` Não aconteceu error por conta do servidor, logo é esperado o FAIL - OK.

``` FAIL 5``` Nesse caso de criação, como ocorreu tudo bem, não aconteceu error 4xx. Logo é esperado o FAIL - OK.

``` FAIL 6``` Nada de errors 5xx. Esperado o FAIL - OK.

``` FAIL 7``` Análogo ao FAIL 5, porém, fora retornado um 201 - OK.

***Análises***

Casos.

1. Se o usuário esquecer algum número do cartão.
2. Se o usuário não colocar os determinados números do cvc.
3. Se o usuário digitar um número inválido de ano de expiração.

Executei os seguintes casos com as determinadas operações. 

![img19](https://user-images.githubusercontent.com/8397519/40285797-a230a82c-5c76-11e8-910e-197028e2d829.png)

Se comparado ao *body* inicial do card, é visto que alterei dois valores no *campo de cartão*, o *número de expiração* do cartão e o deleção de um número do *cvc*. Logo, é gerado um erro de *Bad Request* Como vemos a seguir. 

**Checando o Passed**
![img20](https://user-images.githubusercontent.com/8397519/40285834-1dc3b092-5c77-11e8-8b83-88adc42b2c97.png)

``` PASS 2``` Vemos claramente que um 201 não é retornado, pois não há criação - OK.
``` PASS 3``` É retornado error para o usuário pois ocorre erro - OK.


**Checando o Failed**

![img21](https://user-images.githubusercontent.com/8397519/40285876-83e10348-5c77-11e8-91b7-5a6cc71cb6a3.png)

Temos os 4 primeiros **FAILEDs** deixando claro que não é criado o cartão e não retorna os campos que seriam criados se o **POST** tivesse acontecido com **201 HTTP Code**.

Nos últimos **FAILEDs**, vemos que são erros esperados para um *Bad Request vindo do usuário*

Mais uma vez, deixo como uma possível solução, um melhor redirecionamento de quando for usado a API do Moip, para que seja mostrado ao usuário qual campo ocorreu o erro. Caso também, aconteça a validação de campos por meio de formulários, que o usuário possa preencher os campos obrigatórios não deixando passar se houver um número errado, seja pra qualquer campo. Como visto em algumas situaçes, o form fica em vermelho quando falta algum número.

### Deletando Cartão de Crédito 

Para a ação de deletar, optei por deixá-lo o mais simples possível, tendo em vista que a maioria dos testes padrões foram selecionado nos exemplos anteriores. 

![img22](https://user-images.githubusercontent.com/8397519/40286698-0f7d4acc-5c7f-11e8-8eef-525563311be6.png)

Coloquei quatro testes, três deles sendo conhecidos nosso com um adendo a ser feito sobre um e outro explicarei melhor abaixo.

Você perceberá que ao executar o o método **DELETE**, se tudo ocorrer bem, o retorno do **Body** será vazio, o que pode ocasionar algum erro se você nao atentar para testes que contém o campo **json** requerido. Contudo, apenas retirei a obrigatoriedade do comando de **response** para o **HTTP 200 Code** que é o que é retornado para o **DELETE**. 
Uma possível melhoria para os desenvolvedores, poderia ser o retorno dos campos do **cartão de crédito adicionado** nulos, para ser melhor visível o retorno como também para criar um **Css Model** para ser apresentado ao cliente. 

Por fim, adicionei apenas outro método do **Postman** para me mostrar se o Cartão foi deletado com sucesso.

```
pm.test("Successful DELETE request", function () {
    pm.expect(pm.response.code).to.be.oneOf([200]);
});
```

Vemos o **output** dele no **Passed**. Que praticamente me retorna um "console.log" se o método de retorno foi **OK**. Bem simples, mas bastante visual. 

Quanto a melhoria, mesmo fazendo um novo método de **DELETE** para o mesmo **ID** do cartão anterior, o **output** é bastante satisfatório. Mostrando **404 Not Found** e ativando o teste de 4xx :)

![img26](https://user-images.githubusercontent.com/8397519/40286907-615d541c-5c80-11e8-926c-499fd4a33995.png)
 
Quanto aos erros, é listado os já conhecidos e sendo retornado para o novo teste inserido, o erro que esperávamos.

![img27](https://user-images.githubusercontent.com/8397519/40286948-a065728e-5c80-11e8-8ca4-d2a23722c030.png)

### Consultando Clientes

Os testes para a consulta de cliente, se dá de maneira fácil, via método de API, tanto que na **Sandbox do Moip**, é de fácil uso, sendo os testes bem simplórios. Mesmo utilizando o **Selenium IDE**, não encontrei erros satisfatórios para se por no relatório. Contudo, vou me manter no **Postman** e tentar explorar alguns pontos que não são vistos na **conta-sandbox.moip.com.br**. 

A imagem abaixo, demonstra o ambiente da qual é usado para se obter a lista dos determinados clientes relacionados a conta criada. É dado pelo uso do método **GET**, o primeiro entre os nossos métodos utilizados até agora.

![img29](https://user-images.githubusercontent.com/8397519/40287190-14e8c268-5c82-11e8-8d40-e1db688d436e.png)

Os mesmos aspectos usados para testes, já foram usados antes, sendo que, a reposta que é devolvida nesse campo é diferente, e para fins de demonstração, me limito aos campos de ownId, fullname e id.

Com os três campos citados, podemos checar os minímos detalhes que um usuário pode obter e retornar respostas esperadas. 

Para não deixar passar sem explicar, cito os testes a seguir:

```
//Nome do usuário
   pm.test("Contém o nome no body da mensagem", function () {
    pm.expect(pm.response.text()).to.include("fullname");
   });
  //Id do usuário
   pm.test("Contém o id no body da mensagem", function () {
    pm.expect(pm.response.text()).to.include("id");
   });
   //Id propritário da conta
   pm.test("Contém o id proprietário no body da mensagem", function () {
    pm.expect(pm.response.text()).to.include("ownId");
   });
```

No primeiro **pm** temos a checagem pelo campo do nome completo do Cliente, em seguida a checagem do Id, que é único para cada usuário e ownId, tendo as mesmas caractersticas. 

Logo, obtemos a saida:

![img30](https://user-images.githubusercontent.com/8397519/40287525-3321735e-5c84-11e8-899d-3dce775218d4.png)


```PASS``` Vemos mais uma vez que o formato JSON é retornado contendo o nome do usuário - OK.

```PASS``` Mesmas características da anterior sendo que para o Id - OK.

```PASS``` E também para o Id proprietário - OK.

```PASS 1 ``` Vemos que para o simples GET de usuário, é dado o **200 HTTP Code** 

```PASS 2``` E o OK é retornado.

Para os erros esperados, temos:

![img31](https://user-images.githubusercontent.com/8397519/40287537-4995adc6-5c84-11e8-859c-0772c6e8e58e.png)
 
```FAIL 3``` Nenhum retorno para erro de sistema, logo assim o FAIl é levantado.

```FAIL 4``` Como não houve nenhum 4xx, o FAIL foi levantado. 

Casos

1. Se o usuário procurar https://sandbox.moip.com.br/v2/clientes
2. Se o usuário procurar https://sandbox.moip.com.br/v2/customer

As mesmas respostas são retornadas, não especificando o que há de errado na **URL**, isso para um usuário leigo, pode ser um problema, pois ele não verá que está acessando uma **URL** com algum pequeno erro de sintaxe. 

``` { "ERROR" : "Ops... You are trying to access an invalid URL" } ``` isso é o que é retornado para. 

Uma mensagem mais clara, baseada na rota que o usuário acessa, poderia melhorar a busca do mesmo. 

### Consultando Um Cliente Específico 

É análogo ao exemplo anterior, sendo que um parâmetro é adicionado a **URL** e alguns campos no **Test**.

Fiz a busca a um cliente meu cadastrado na base. Como já sei o **id**, **ownId** e **fullname**, fiz a busca usando por parâmetro o preenchimento desses campos.

Segue abaixo a imagem, ilustrando melhor o que fora explicado. 

![img32](https://user-images.githubusercontent.com/8397519/40287786-2c100ca4-5c86-11e8-9ea5-74bc498e7bb8.png)

Vale lembrar que possuo já o **ID** de busca na **URL** visto em exemplos anteriores. 

Explicando o código, nada mas faço do que percorrer pelo **Body** e checar pelos campos que desejo encontrar se são válidos, uma vez que são válidos, vejo se eles batem com a **String** que eu possuo, uma vez que isso acontece, é me retornado o que eu espero.

As demais linhas, possuem os testes de padrão, usados em toda a coleção.

![img33](https://user-images.githubusercontent.com/8397519/40287838-91082b8c-5c86-11e8-9d58-22661cf2bc83.png)

Os ```PASSES``` são bastante lúcidos, pois batem com os campos que foram inseridos no **Tests**

Já quanto aos erros, são erros simples, retornado no output de listagem de usuários.

![img34](https://user-images.githubusercontent.com/8397519/40287900-ec569528-5c86-11e8-9aa8-5a0cc4bd25b4.png)

Casos

1. Se um usuário buscar pelo **fullname** do usuário na **URL**, o que é retornado?
2. Se um usuário buscar pelo **ownId** do usuário na **URL**, o que é retornado?

Para os casos citados acima, um **JSON** é gerado no **Body** de testes e é demontrado que não existe determinada fonte para aquele usuário. A tratativa para um melhoramento disso, é que a implementação para busca com **ownId** e **fullname** possam ser aplicadas, pois se torna mais fácil. Visto também, que o erro de retorno, possa ser mais amigável, como: ``` O formato de busca é inválido para o campo``` ou redireciona para uma rota anterior da qual a busca está sendo feita.





## Criando Pedidos

Na área de testes, é regular o uso de **schemas** para testes de **APIS**. O **Postman** por sua vez nos permite usar também esse conotação, visto que é baseado **JSON** e pode ser exportado para outras plataformas de testes, por exemplo, na a **gema** ou **gems** como prefirir , referente ao **Ruby** e **Ruby on Rails** que é usado juntamente  com o **rspec**.

Como a colection para **Pedidos** é menor se comparado ao de **Criação de Clientes e Pagamentos**, usarei nessa seção a abordagem dita. Ela é bem simples, pois é herdada do **JSON** passado como campo no **body** da **API**. Segue a imagem abaixo:

![pedido01](https://user-images.githubusercontent.com/8397519/40458716-e9a7845a-5ed3-11e8-8c37-bfe11141ea5f.png)

Essa abordagem, pode soar um pouco suja, pois não desejamos ter inúmeros **schemas JSON** na nossa suíte de testes. O **Postman** nos permite criar outro esquemas, usando **variáveis**. Porém, para exemplo, deixarei da maneira que se encontra.

Como podemos ver, temos o uso de outra propriedade nos nossos testes, o **tv4**. O **tv4** nada mais é que uma bibliotaca, da qual nos permite escrever testes para e testar se a **API** responde ao nosso **schema JSON** de testes.

O mesmo princípio de testes é feito, checando pela aparição dos campos como também criação deles no **response body** da requisição. Esse valor é salvo numa variável chamada **pedido**, e o **tv4** faz essa checagem retornando ao teste esperado! 

***Observação**, vale lembrar também que a **URL** usada no método, é diferente*. 

![pedidosbody](https://user-images.githubusercontent.com/8397519/40458761-294229ee-5ed4-11e8-9500-0bceab4df3e0.png)

O **body JSON** passado para criação do **Pedido** na **API** é totalmente baseado no guia de referência.
Repeti o mesmo código, tendo mudado apenas o **ID** do usuário. Logo, a verificação fora feita somente a nível não aprofundado. A tratativa é feita somente para campos com **strings** e **integers**, ficando a mercê de vulnerabilidades de **injections code**. Mas como estamos testando simples validações, para nosso caso, a abstração da camada de segurança é válida. 

**Retorno do teste**

![pedidovalido](https://user-images.githubusercontent.com/8397519/40458925-00b0782c-5ed5-11e8-8363-b35e9310c835.png)

Por fim, vemos que para o nosso simples teste, o **HTTP Code 201** fora retornado, confirmando a criação do **Pedido** como também o **PASS** para o nosso teste.

## Consultar Pedidos ##

A consulta de pedidos, também é feita de maneira simples. Para a verificação, dentro do **Body** do nosso response de **Criação de Pedido**, coletamos o **ID** gerado, e fazemos a busca por ele via método **GET**. 

![consultarpedidos](https://user-images.githubusercontent.com/8397519/40459078-d129920e-5ed5-11e8-9344-2f9aff7ccbf4.png)

Como podemos ver, reaproveitamos nosso código de teste do exemplo anterior, e também o da checagem do **HTTP Code 200**, para checar se a existência do pedido é válida. Assim, concluímos que tanto para a criação e para consulta, temos uma resposta válida da **API**. 

E para uma checagem mais geral, acerca de todos os **pedidos**, excluímos apenas o validador para determinado pedido e fazemos a busca em cima da rota raiz para padidos.

![pedidosnogeral](https://user-images.githubusercontent.com/8397519/40459199-76e62a04-5ed6-11e8-96d9-4bee3264a293.png)

Assim também, temos o teste simples de retorno do método GET para o orders. Sendo que por sua vez, o retorno no **body**, não tenha retornado como demonstrado na **referência da API**. Tentei encontrar algum erro na criação ou até na busca, mas não fui capaz de encontrar, tendo em vista que os pedidos estavam sendo criados normalmente e podendo ser checados unitariamente.

Segue abaixo a imagem descritiva:

![getemtodos](https://user-images.githubusercontent.com/8397519/40459327-25c44880-5ed7-11e8-9a60-7047f02a3498.png)

Por não possuir maturidade o suficiente em testes, não pude encontrar o que fiz de errado nos processos, como justifiquei no parágrafo anterior. Mas segue a imagem em destaque, podendo acontecer de alguém ter passado o mesmo.

## Pagamentos ## 

Referente ao **pagamento** necessitamos do identificador do nosso pedido. Com ele em mãos, podemos via método **POST** adicionar o cartão de crédito para o pagamento. É indispensável também, o uso do **hash** do cartão gerado pelo **Moip.js**.

Tendo em vista alguns problemas, não consegui dar continuidade nessa fase da do teste.

Obtive alguns erros na checagem do hash do cartão, tentei algumas outras alternativas, porém não alcançei sucesso.

Um exemplo, é que mesmo aderindo ao modelo oferecido pela **API** e a criação do pagamento com o uso de um **novos hashes** gerados pelo **Moip.js**, o método **POST** sempre retornava o erro de não conseguir fazer a checagem.

Sou grato a toda equipe pela documentação da **API**. Achei muito clara e bastante satisfatória. Se comparado a algumas que conheci, essa é uma das mais bem escritas. Parabéns a todos os envolvidos.



## Deployment

Qualquer sistema operacional que possa executar o **Postman**

## Construído com:

* [Postman](https://www.getpostman.com/) - Ferramenta principal usada


## Autor

* **Diego Amaral** 

## Agradecimentos

* Agradecimento a toda equipe Moip Pagamentos 
* Mariana Santana

