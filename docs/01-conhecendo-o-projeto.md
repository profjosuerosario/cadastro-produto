# Tutorial 01 --- Conhecendo o projeto Cadastro de Produtos

**Área:** Desenvolvimento Back-end\
**Tecnologias previstas:** TypeScript, Node.js e Express\
**Organização do código:** MVC e Repository\
**Armazenamento inicial:** array em memória

------------------------------------------------------------------------

## 1. Objetivos de aprendizagem

Ao concluir este tutorial, você deverá ser capaz de:

-   compreender o objetivo do sistema Cadastro de Produtos;
-   identificar as funcionalidades que serão desenvolvidas;
-   reconhecer as tecnologias principais do projeto;
-   compreender, de forma introdutória, como as partes de uma aplicação
    back-end se relacionam;
-   entender a sequência de desenvolvimento que será seguida nos
    próximos tutoriais.

Neste primeiro tutorial, vamos conhecer o projeto antes de instalar
ferramentas ou escrever código.

## 2. Pré-requisitos

Não é necessário ter experiência prévia com TypeScript, Node.js,
Express, MVC ou Repository.

É útil saber utilizar arquivos e pastas no computador e ter curiosidade
para entender como uma aplicação funciona.

## 3. O que será construído?

Vamos desenvolver um sistema back-end de cadastro de produtos. Um
produto terá quatro informações:

  Campo          Tipo previsto   Significado
  -------------- --------------- ----------------------------------
  `id`           `number`        Identificador do produto
  `nome`         `string`        Nome do produto
  `preco`        `number`        Preço do produto
  `quantidade`   `number`        Quantidade disponível em estoque

Por exemplo, o sistema poderá representar um produto desta forma:

``` json
{
  "id": 1,
  "nome": "Teclado",
  "preco": 89.9,
  "quantidade": 12
}
```

Esse exemplo representa os dados de um produto em formato JSON. Ainda
não estamos programando o sistema; estamos apenas visualizando como as
informações podem ser representadas.

### 3.1 Funcionalidades previstas

Ao final da sequência de tutoriais, o sistema deverá permitir:

1.  cadastrar um produto;
2.  listar todos os produtos;
3.  consultar um produto pelo identificador;
4.  atualizar os dados de um produto;
5.  excluir um produto.

Essas operações são frequentemente agrupadas pela sigla **CRUD**:

-   **Create:** criar ou cadastrar;
-   **Read:** consultar ou listar;
-   **Update:** atualizar;
-   **Delete:** excluir.

Não precisamos memorizar a sigla agora. Vamos implementar e compreender
cada operação ao longo do projeto.

## 4. O que é uma aplicação back-end?

Uma aplicação back-end é a parte de um sistema que executa no servidor e
trata regras e operações necessárias para atender às solicitações de um
cliente.

Neste projeto, o cliente poderá ser uma ferramenta de testes HTTP, como
o Postman. O cliente enviará uma solicitação; o servidor processará essa
solicitação e devolverá uma resposta.

Por exemplo, o cliente poderá solicitar a lista de produtos. O servidor
consultará os dados disponíveis e responderá com uma lista em JSON.

``` mermaid
sequenceDiagram
    participant C as Cliente HTTP
    participant S as Servidor back-end
    participant D as Dados em memória
    C->>S: Solicita a lista de produtos
    S->>D: Consulta os produtos
    D-->>S: Devolve os produtos
    S-->>C: Responde com JSON
```

Esse é um fluxo conceitual. Vamos construir e testar cada parte nos
próximos tutoriais.

## 5. Tecnologias do projeto

### 5.1 TypeScript

TypeScript será a linguagem usada para escrever o código do projeto. Ela
acrescenta recursos de tipagem ao JavaScript, ajudando a identificar
determinados erros durante o desenvolvimento.

### 5.2 Node.js

Node.js será o ambiente de execução do servidor. Ele permite executar
código JavaScript fora do navegador.

### 5.3 Express

Express será utilizado para organizar o servidor HTTP e, mais adiante,
definir como ele responderá às requisições.

### 5.4 JSON

JSON é um formato textual usado para representar e trocar dados
estruturados. Será o formato utilizado nas comunicações do nosso
sistema.

### 5.5 Array em memória

Inicialmente, os produtos serão armazenados em um array mantido pelo
próprio programa. Isso é adequado para aprender os fundamentos sem
precisar configurar um banco de dados.

Existe uma limitação importante: **quando o processo do servidor for
encerrado ou reiniciado, os produtos cadastrados serão perdidos**. Essa
limitação é intencional nesta etapa didática.

Não usaremos banco de dados nesta primeira versão.

## 6. Como organizaremos o código?

À medida que o projeto evoluir, separaremos as responsabilidades em
componentes. Isso evita concentrar toda a aplicação em um único arquivo.

### 6.1 Model

O **Model** representará os produtos no código. Neste projeto, criaremos
uma classe TypeScript chamada `Produto`, com atributos tipados e um
construtor.

Uma classe é uma estrutura usada para definir características e
comportamentos de objetos. Vamos estudar classes e objetos antes de
aprofundar a organização das demais partes.

### 6.2 Repository

O **Repository** será responsável por manter o array de produtos e
realizar operações sobre esses dados, como consultar, incluir, atualizar
e excluir.

As outras partes da aplicação não deverão manipular o array diretamente.

### 6.3 Controller

O **Controller** será responsável por receber e interpretar os dados de
uma requisição, solicitar as operações necessárias ao Repository e
preparar a resposta HTTP.

### 6.4 Routes

As **Routes** definirão os caminhos e métodos HTTP disponíveis. Elas
encaminharão as requisições para os Controllers correspondentes.

### 6.5 Servidor

O arquivo do servidor configurará o Express, registrará os recursos
necessários e iniciará a aplicação para que ela possa receber
requisições.

## 7. Visualizando a arquitetura planejada

O diagrama mostra o fluxo principal entre as partes da aplicação:

``` mermaid
flowchart TD
    A[Cliente HTTP] --> B[Routes]
    B --> C[Controller]
    C --> D[Repository]
    D --> E[Array de produtos em memória]
    E --> D
    D --> C
    C --> B
    B --> A
    M[Model: classe Produto] -. representa os dados .-> D
```

Leia o fluxo de cima para baixo:

1.  O cliente envia uma requisição HTTP.
2.  Uma rota identifica o caminho e o método da requisição.
3.  O Controller coordena o tratamento da solicitação.
4.  O Repository executa a operação necessária no array.
5.  O resultado retorna ao Controller, que prepara a resposta ao
    cliente.

O Model define a representação dos produtos. Ele não é uma etapa isolada
que precisa ser executada em toda requisição.

**Não é necessário decorar o diagrama neste momento.** A finalidade é
conhecer os nomes e ter uma visão geral do que construiremos.

## 8. Como será o desenvolvimento?

O projeto será construído progressivamente. Cada etapa aproveitará o que
foi desenvolvido antes.

  -----------------------------------------------------------------------
  Tutorial                            O que faremos
  ----------------------------------- -----------------------------------
  01 --- Conhecendo o projeto         Entender o objetivo, as
                                      funcionalidades e a arquitetura
                                      planejada

  02 --- Preparação do ambiente       Verificar Node.js e npm, criar o
                                      projeto e configurar TypeScript

  03 --- Primeiro servidor            Configurar o Express e iniciar o
                                      servidor

  04 --- Introdução às rotas HTTP     Criar rotas simples e responder com
                                      JSON

  05 --- Model, classes e orientação  Criar a classe `Produto`
  a objetos                           

  06 --- Repository e arrays          Armazenar e manipular produtos em
                                      memória

  07 --- Introdução ao MVC            Criar o Controller e compreender
                                      suas responsabilidades

  08 --- Integração das camadas       Conectar Routes, Controller e
                                      Repository

  09 --- Consultar produtos           Implementar a listagem e a consulta
                                      por identificador

  10 --- Cadastrar produtos           Receber os dados e implementar o
                                      cadastro

  11 --- Atualizar e excluir produtos Implementar a atualização e a
                                      exclusão

  12 --- Testes e revisão final       Testar as operações e revisar a
                                      arquitetura
  -----------------------------------------------------------------------

## 9. O que não faremos nesta primeira versão?

Para manter o projeto adequado ao aprendizado dos fundamentos, não
utilizaremos inicialmente:

-   banco de dados;
-   ORM ou ODM;
-   autenticação e autorização;
-   interface front-end;
-   contêineres;
-   microsserviços;
-   camadas adicionais sem necessidade didática.

Esses recursos não são o foco desta primeira versão. O objetivo é
compreender a organização básica de uma aplicação back-end antes de
acrescentar outras tecnologias.

## 10. Atividades de compreensão

Responda às questões com suas próprias palavras. Consulte o tutorial
quando necessário.

1.  Qual é o objetivo do sistema Cadastro de Produtos?
2.  Quais são os quatro atributos previstos para um produto?
3.  Quais são as cinco funcionalidades que o sistema deverá oferecer?
4.  Qual é a função do Node.js neste projeto?
5.  Para que utilizaremos o Express?
6.  Qual será a responsabilidade do Repository?
7.  Por que não devemos permitir que o Controller manipule diretamente o
    array?
8.  O que acontecerá com os produtos cadastrados quando o servidor for
    reiniciado?
9.  Qual é a diferença entre Model, Controller e Routes?
10. Por que o projeto será desenvolvido em etapas, em vez de escrever
    todo o código de uma vez?

## 11. Critérios de conclusão

Antes de seguir para o próximo tutorial, confirme que você consegue:

-   [ ] explicar o objetivo do sistema;
-   [ ] identificar os atributos de um produto;
-   [ ] listar as funcionalidades previstas;
-   [ ] explicar, de maneira introdutória, o papel de TypeScript,
    Node.js e Express;
-   [ ] distinguir as responsabilidades de Model, Repository, Controller
    e Routes;
-   [ ] explicar por que os dados serão perdidos ao reiniciar o
    servidor.

Neste tutorial, não era necessário instalar programas nem escrever
código.

## 12. Resumo e próximo passo

Você conheceu o sistema que será desenvolvido, seus atributos, suas
funcionalidades, as tecnologias escolhidas e a arquitetura planejada.

A ideia central é dividir a aplicação em partes com responsabilidades
distintas. O Repository cuidará dos dados em memória; o Controller
coordenará o tratamento das solicitações; as Routes definirão os
caminhos HTTP; o Model representará os produtos; e o servidor
configurará o Express.

**Próximo tutorial --- 02: Preparação do ambiente.** Vamos verificar se
Node.js e npm estão disponíveis, criar a pasta do projeto, instalar as
dependências e configurar o TypeScript.
