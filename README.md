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

Logo abaixo do botão **SEND** vemos o status da nossa requisição, sendo demonstradao como ``` Status 201 Created ```.
É nos mostrado que o nosso cliente foi cadastrado com sucesso e **4** testes passaram na nossa bateria. Vemos que são **4/10** o que pode soar estranho para algunns. Porém, os testes que passam, são exatamente os que importam para nós.

``PASS`` "Contém o id proprietário no body da mensagem". Vemos que o ownId fora preenchido.  heavy_check_mark
```PASS``` "Contém o nome no body da mensagem". Representado pelo nosso fullname. Mais uma vez, é o que esperávamos.  heavy_check_mark
```PASS``` Checagem do formato JSON, vemos que funcionou.  heavy_check_mark
``PASS`` Além do 201, vemos que um OK também é enviado. heavy_check_mark




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
