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

### Iniciando

Uma vez que foram executados os passos acima, é necessário que seja compreendido o funcionamento da API. Com isso, é disponibilizado uma referência para melhor entendimento.

```
https://dev.moip.com.br/v2.0/reference
```
Visto isso, faça com que o entedimento de Autenticação esteja claro, pois é necessário o uso de Token e Chaves para conexão com a API. 
Assumindo o passo anterior, ao abrirmos o Postman, teremos o seguinte ambiente:

![img1](https://user-images.githubusercontent.com/8397519/40283585-e9a01244-5c56-11e8-9191-f97b640fdd7d.png)

Reforçando, veja que meu Username é meu token enquanto que a senha, é minha chave. 

### Overview

Nas seguintes imagens, comentarei simples campos que são preenchidos com maiores detalhes.

```
Body
```
![img2](https://user-images.githubusercontent.com/8397519/40283656-11299122-5c58-11e8-9ac9-27772954fe5d.png)

No campo acima, estou criando um usuário simples, baseado nas mandatórias da API para testes. Isso inclui responses envolvendo json e códigos HTTP.

*O Content-Type está setado como também o application/json para execução.*

```
Pre-request Script
```
![img3](https://user-images.githubusercontent.com/8397519/40283706-eda64906-5c58-11e8-96a7-072f3bdce0c8.png)

**Pre-request Script** são pedaços de códigos que são executados antes de um request. 
Como será utilizado uma feature do Postman, o **Postman BDD**, é necessário definir o código acima para execução. 

**Postman Behavior Driven Development (Postman BDD)**

O Postman BDD permite usar a sintaxe do BDD para estruturar os testes e usar a sintax que é executada no postman de maneira que não atrapalhe a execução para os assertions. Abaixo é listado um simples comando de como funciona:

```
it(‘should be a 200 response’, () => {
 response.should.have.status(200);                           
});
```
Checa se o response executado por um método GET, POST, DELETE, etc é a mensagem HTTP 200, Successful Request. 

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

Nessa aba, estão listados testes para checagem do nosso método que será criado por meio da URL concebida pela API.

![img5](https://user-images.githubusercontent.com/8397519/40284091-cf418b2c-5c5f-11e8-9490-8518acff0c59.png)

Podemos enxergar na primeira linha, uma chamada do BDD, basta apenas copiar e colar no topo do arquivo.

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
O exemplo que vemos acima é listado nas primeiras linhas do arquivo de teste e ele nada mais é do que uma simples checagem. Como estamos trabalhando com o formato JSON, é importante checar se o que é retornado é o que é esperado como response.

Ao criarmos um usuário, o output esperado é de um código **201 HTTP** via método POST. Por isso esperamos por um código 201. Esse padrão é observado para o padrão BDD.

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

```PASS 4 ``` Retornou um erro por parte do cliente. Nesse campo, se visto em comparação com o último FAIL, é dado pelo simples erro de sintax, mas que pode passar sem ser visto por alguns QA do time. Porém ele retorna como esperado. Correto.

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

```PASS``` 1. Recebe informações do cliente - Deve retornar uma resposta válida - Vemos que fora retornada, OK.

```PASS``` 3. Recebe informações do cliente - Envia um OK como resposta - Foi enviado o status, OK.

Vemos por meio de testes simples, o que é retornado. Como solução ou melhoramento para os testes que passarão, não tenho nenhuma em mente, acredito que elas satisfazem o que é necessário para uma boa navegação do cliente.

**Checando o Failed**

![img16](https://user-images.githubusercontent.com/8397519/40285616-dfbfab36-5c74-11e8-9b8e-39d4ef798153.png)

``` FAIL ``` 2. Vemos que o 201 retorna para o usuário, logo o não retornar dele, garante que o teste falhe - OK.

``` FAIL ``` 4. Não aconteceu error por conta do servidor, logo é esperado o FAIL - OK.

``` FAIL ``` 5. Nesse caso de criação, como ocorreu tudo bem, não aconteceu error 4xx. Logo é esperado o FAIL - OK.

``` FAIL ``` 6. Nada de errors 5xx. Esperado o FAIL - OK.

``` FAIL ``` 7. Análogo ao FAIL 5, porém, fora retornado um 201 - OK.

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

``` PASS``` 2. Vemos claramente que um 201 não é retornado, pois não há criação - OK.
``` PASS``` 3. É retornado error para o usuário pois ocorre erro - OK.


**Checando o Failed**

![img21](https://user-images.githubusercontent.com/8397519/40285876-83e10348-5c77-11e8-91b7-5a6cc71cb6a3.png)

Temos os 4 primeiros *FAILEDs* deixando claro que não é criado o cartão e não retorna os campos que seriam criados se o *post* tivesse acontecido com *201 http code*.

Os últimos *FAILEDs* vemos que são erros esperados para um *Bad Request vindo do usuário*

Mais uma vez, deixo como uma possível solução, um melhor redirecionamento de quando for usado a API do Moip, para que diga ao usuário qual campo ocorreu um error, como também, aconteça a validação de campos por meio de formulários, obrigando o usuário a preencher os campos obrigatórios e não deixando passar se houver um número errado, mostrando mais uma vez qual o campo isso acontece. 



End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc
