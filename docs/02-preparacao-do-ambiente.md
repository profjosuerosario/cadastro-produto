# Tutorial 02 — Preparação do ambiente

**Projeto:** Sistema de Cadastro de Produtos  
**Tecnologias:** TypeScript, Node.js, Express e npm  
**Ambiente de referência:** Windows com PowerShell  
**Nível:** Iniciante

---

## 1. Objetivos de aprendizagem

Ao concluir este tutorial, você deverá ser capaz de:

- verificar se o Node.js e o npm estão instalados;
- criar a pasta do projeto;
- inicializar um projeto com npm;
- instalar as dependências necessárias;
- configurar o TypeScript;
- configurar uma ferramenta para executar TypeScript durante o desenvolvimento;
- explicar a finalidade dos principais arquivos de configuração.

Neste tutorial, vamos preparar o ambiente. **Ainda não vamos criar as rotas, o Model `Produto`, o Repository, os Controllers ou as operações CRUD.**

## 2. Pré-requisitos

Você precisará de:

- Windows com PowerShell;
- Node.js instalado;
- conexão com a internet para baixar os pacotes;
- um editor de código, preferencialmente Visual Studio Code.

Não é necessário instalar o TypeScript globalmente. Vamos instalar as ferramentas como dependências do próprio projeto.

## 3. O que vamos construir?

Ao final, a pasta terá uma estrutura semelhante a esta:

```text
cadastro-produtos/
├── node_modules/
├── package.json
├── package-lock.json
└── tsconfig.json
```

O diretório `node_modules` será criado pelo npm e conterá os pacotes instalados. Ele não deve ser editado manualmente.

## 4. Conceitos necessários

### 4.1 Node.js

O Node.js permite executar JavaScript fora do navegador. Neste projeto, ele será o ambiente de execução do servidor back-end.

### 4.2 npm

O npm é o gerenciador de pacotes que acompanha a instalação usual do Node.js. Ele permite instalar bibliotecas e registrar as dependências do projeto.

### 4.3 TypeScript

TypeScript acrescenta recursos de tipagem ao JavaScript. O código TypeScript normalmente é transformado em JavaScript antes de ser executado pelo Node.js, embora ferramentas de desenvolvimento possam facilitar esse processo.

### 4.4 Express

Express é uma biblioteca para construir aplicações web e servidores HTTP com Node.js. Vamos instalá-la agora, mas a configuração do servidor será feita no Tutorial 03.

### 4.5 Dependências de produção e de desenvolvimento

- **Dependência de produção:** necessária para a aplicação funcionar, como o Express.
- **Dependência de desenvolvimento:** auxilia na escrita, verificação ou execução do código durante o desenvolvimento, como TypeScript e `tsx`.

## 5. Passo a passo

### Etapa 1 — Verificar o Node.js

Abra o PowerShell. Você pode fazer isso pelo menu Iniciar do Windows.

Execute:

```powershell
node --version
```

O comando deve mostrar um número de versão, por exemplo:

```text
v22.x.x
```

O número exibido no seu computador pode ser diferente. O importante é que o comando funcione e que você utilize uma versão LTS do Node.js compatível com as ferramentas do projeto.

Se aparecer uma mensagem dizendo que `node` não é reconhecido, o Node.js pode não estar instalado ou não estar configurado no `PATH`.

### Etapa 2 — Verificar o npm

No mesmo PowerShell, execute:

```powershell
npm --version
```

O comando deve mostrar a versão do npm.

Se `node --version` funcionar, mas `npm --version` não, verifique a instalação do Node.js e abra um novo terminal após corrigir a instalação.

### Etapa 3 — Criar a pasta do projeto

Escolha um local para guardar seus projetos. Neste exemplo, usaremos a pasta `Documents`.

No PowerShell:

```powershell
Set-Location "$HOME\Documents"
New-Item -ItemType Directory -Name "cadastro-produtos"
Set-Location ".\cadastro-produtos"
```

O primeiro comando entra na pasta Documentos do usuário atual. O segundo cria a pasta do projeto e o terceiro entra nela.

> Se a pasta `cadastro-produtos` já existir, não execute novamente o comando de criação. Entre na pasta existente com `Set-Location`.

Confira o diretório atual:

```powershell
Get-Location
```

O caminho exibido deve terminar com `cadastro-produtos`.

### Etapa 4 — Inicializar o projeto npm

Execute dentro da pasta `cadastro-produtos`:

```powershell
npm init -y
```

Esse comando cria o arquivo `package.json` com configurações iniciais.

O parâmetro `-y` aceita as respostas padrão para as perguntas de inicialização. Você poderá editar os dados do arquivo posteriormente.

Confira o conteúdo:

```powershell
Get-Content .\package.json
```

O `package.json` registra informações do projeto, scripts e dependências. O conteúdo inicial pode variar conforme a versão do npm.

### Etapa 5 — Instalar o Express

Ainda dentro da pasta do projeto, execute:

```powershell
npm install express
```

O npm instala o Express e registra a dependência no `package.json`. Também cria ou atualiza o `package-lock.json`, que registra a árvore de dependências instalada.

**Por que instalar agora?** O Express faz parte das tecnologias definidas para o projeto. Vamos utilizá-lo para construir o servidor no próximo tutorial.

### Etapa 6 — Instalar as ferramentas de desenvolvimento

Execute:

```powershell
npm install --save-dev typescript tsx @types/node @types/express
```

Esses pacotes têm finalidades diferentes:

| Pacote | Finalidade |
|---|---|
| `typescript` | Fornece o compilador e as ferramentas do TypeScript. |
| `tsx` | Permite executar arquivos TypeScript durante o desenvolvimento. |
| `@types/node` | Fornece definições de tipos para APIs do Node.js. |
| `@types/express` | Fornece definições de tipos para o Express. |

O `--save-dev` registra os pacotes em `devDependencies`, pois são ferramentas de desenvolvimento ou definições de tipos.

### Etapa 7 — Criar a configuração do TypeScript

Execute:

```powershell
npx tsc --init
```

O comando `npx` permite executar uma ferramenta disponibilizada pelos pacotes do projeto. Neste caso, ele executa o compilador TypeScript instalado localmente.

O comando cria o arquivo `tsconfig.json`, que configura como o TypeScript analisa e transforma os arquivos.

Abra `tsconfig.json` no editor. O arquivo criado pode conter muitas opções comentadas. Para este projeto introdutório, substitua seu conteúdo pelo seguinte:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "rootDir": "./src",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules", "dist"]
}
```

Salve o arquivo.

#### O que significam as principais opções?

- `target`: define a versão de JavaScript de destino da compilação.
- `module`: define o sistema de módulos usado pelo projeto.
- `moduleResolution`: define como o TypeScript localiza os módulos importados.
- `rootDir`: indica a pasta de origem dos arquivos TypeScript da aplicação.
- `outDir`: indica a pasta onde os arquivos JavaScript compilados serão colocados.
- `strict`: ativa verificações rigorosas de tipos.
- `esModuleInterop`: facilita a interoperabilidade com pacotes que usam formatos de módulos diferentes.
- `skipLibCheck`: evita verificar internamente os arquivos de declaração de tipos das dependências.
- `include`: informa quais arquivos TypeScript fazem parte da compilação.
- `exclude`: exclui diretórios da busca de arquivos de entrada.

As pastas `src` e `dist` ainda não precisam existir. Vamos criar a pasta `src` no próximo tutorial, quando começarmos a construir o servidor. `dist` será o destino dos arquivos compilados.

### Etapa 8 — Configurar o projeto como CommonJS ou ES Modules?

Neste tutorial, usaremos **ES Modules com `NodeNext`**, uma configuração alinhada ao sistema de módulos moderno do Node.js.

Para deixar essa escolha explícita, adicione `"type": "module"` ao `package.json`. Não substitua as dependências que o npm já registrou. Edite o arquivo para incluir a propriedade no nível principal, por exemplo:

```json
{
  "name": "cadastro-produtos",
  "version": "1.0.0",
  "description": "Sistema didático de cadastro de produtos",
  "type": "module",
  "scripts": {},
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.21.2"
  },
  "devDependencies": {
    "@types/express": "^5.0.0",
    "@types/node": "^22.0.0",
    "tsx": "^4.0.0",
    "typescript": "^5.0.0"
  }
}
```

**Atenção:** esse bloco ilustra a estrutura do arquivo, não as versões exatas instaladas no seu computador. Preserve os números de versão gerados pelo npm e mantenha todas as dependências registradas. Apenas acrescente `"type": "module"` e, se desejar, atualize `description`.

A propriedade `"type": "module"` informa ao Node.js que os arquivos `.js` do projeto devem ser interpretados como módulos ES. Isso afeta a forma de usar `import` e `export`.

### Etapa 9 — Criar scripts úteis

No `package.json`, localize a propriedade `"scripts"` e configure-a assim:

```json
"scripts": {
  "dev": "tsx watch src/server.ts",
  "build": "tsc",
  "start": "node dist/server.js"
}
```

Não crie uma segunda propriedade `scripts`: edite a que já existe. Mantenha as vírgulas do JSON corretamente.

O que cada script fará:

- `npm run dev`: executará o arquivo `src/server.ts` e observará alterações para reiniciar a execução durante o desenvolvimento.
- `npm run build`: compilará os arquivos TypeScript incluídos na configuração.
- `npm start`: executará o JavaScript compilado em `dist/server.js`.

Esses scripts serão úteis nos próximos tutoriais. **Ainda não execute `npm run dev` nem `npm start`**, pois o arquivo `src/server.ts` ainda não foi criado.

### Etapa 10 — Conferir as dependências e a configuração

Execute:

```powershell
npm ls --depth=0
```

O comando lista as dependências de primeiro nível instaladas no projeto.

Depois, verifique a versão do compilador:

```powershell
npx tsc --version
```

E valide a configuração do TypeScript:

```powershell
npx tsc --noEmit
```

Neste momento, a validação pode informar que não encontrou arquivos de entrada, pois ainda não existe a pasta `src` com arquivos TypeScript. Isso é esperado nesta etapa. O objetivo principal aqui é preparar a configuração; a compilação será validada com código no próximo tutorial.

## 6. Arquivos criados ou modificados

Ao terminar, você deverá ter:

| Arquivo ou pasta | Para que serve |
|---|---|
| `package.json` | Registra os dados, scripts e dependências do projeto. |
| `package-lock.json` | Registra versões resolvidas e a árvore de dependências. |
| `node_modules/` | Armazena os pacotes instalados pelo npm. |
| `tsconfig.json` | Configura a análise e a compilação do TypeScript. |

Não crie manualmente `package-lock.json` ou `node_modules`. O npm administra esses itens.

## 7. Como executar e verificar

Neste tutorial ainda não existe um servidor para executar. Faça estas verificações:

1. `node --version` mostra a versão do Node.js.
2. `npm --version` mostra a versão do npm.
3. `npm ls --depth=0` lista as dependências sem indicar erros de instalação.
4. `npx tsc --version` mostra a versão do TypeScript.
5. `package.json` contém Express, as ferramentas de desenvolvimento e os scripts.
6. `tsconfig.json` contém as opções de configuração apresentadas.

## 8. Problemas comuns e soluções

### `node` ou `npm` não é reconhecido

- Confirme se o Node.js foi instalado.
- Feche e abra novamente o PowerShell.
- Verifique se o diretório de instalação do Node.js está no `PATH`.
- Execute novamente os comandos de versão.

### `npm install` apresenta erro de rede

- Confira a conexão com a internet.
- Se estiver no laboratório, verifique se a rede ou o proxy da instituição bloqueia o acesso ao registro do npm.
- Leia as últimas linhas da mensagem de erro antes de repetir a instalação.

### Erro de sintaxe no `package.json`

O arquivo precisa ser JSON válido. Verifique se:

- cada chave e cada texto estão entre aspas duplas;
- não há vírgula sobrando depois do último item de um objeto;
- existe somente uma propriedade `"scripts"`;
- as propriedades `"dependencies"` e `"devDependencies"` não foram apagadas acidentalmente.

### `npx tsc --init` diz que o comando não existe

Confira se a etapa de instalação das ferramentas de desenvolvimento terminou sem erros. Execute novamente:

```powershell
npm install --save-dev typescript tsx @types/node @types/express
```

Depois tente:

```powershell
npx tsc --init
```

Se `tsconfig.json` já existir, não o recrie sem necessidade: edite o arquivo existente.

### O `npm run dev` informa que não encontra `src/server.ts`

Esse arquivo será criado no Tutorial 03. Portanto, essa mensagem é esperada se você tentar executar o script antes de concluir o próximo tutorial.

## 9. Exercícios para os alunos

1. Explique, com suas palavras, a diferença entre Node.js e npm.
2. Qual é a finalidade do arquivo `package.json`?
3. Qual é a diferença entre `dependencies` e `devDependencies`?
4. Para que serve o arquivo `tsconfig.json`?
5. O que o comando `npx tsc --init` faz?
6. Qual é a função do pacote `tsx`?
7. Por que não devemos editar manualmente os arquivos dentro de `node_modules`?
8. O que acontecerá se tentarmos iniciar o servidor antes de criar `src/server.ts`?

## 10. Critérios de conclusão

Marque cada item após verificar no seu computador:

- [ ] Node.js e npm respondem aos comandos de versão.
- [ ] A pasta `cadastro-produtos` foi criada.
- [ ] O projeto foi inicializado com `npm init -y`.
- [ ] O Express foi instalado.
- [ ] As ferramentas de desenvolvimento foram instaladas.
- [ ] O arquivo `tsconfig.json` foi configurado.
- [ ] O `package.json` contém `"type": "module"` e os scripts planejados.
- [ ] Você consegue explicar a finalidade dos principais arquivos.

## 11. Resumo e preparação para o próximo tutorial

Neste tutorial, preparamos o ambiente do projeto Cadastro de Produtos com Node.js, npm, Express e TypeScript. Também configuramos os módulos ES e os scripts que serão usados durante o desenvolvimento.

Ainda não criamos funcionalidades de cadastro de produtos nem as camadas MVC.

No **Tutorial 03 — Primeiro servidor**, vamos criar `src/server.ts`, configurar o Express, iniciar o servidor HTTP e verificar se ele está funcionando.

---

**Documento de referência:** `01-planejamento.md`  
**Próximo tutorial:** `03 - Primeiro servidor`
