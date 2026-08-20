[![Maven CI](https://github.com/FabioGVL/SicrediAPI/actions/workflows/maven.yml/badge.svg)](https://github.com/FabioGVL/SicrediAPI/actions/workflows/maven.yml)

# Automação de Testes de API - Sicredi API

## Escopo do Produto

Os testes abaixo visam garantir a funcionalidade correta e a integridade dos dados fornecidos pela API. Todos os testes foram realizados na linguagem Java, utilizando ferramentas de automação e validação de contratos, com relatórios gerados via Maven Surefire Report para análise e melhoria contínua do sistema.

## Escopo do Teste

A estratégia foca em garantir a funcionalidade correta e a integridade dos principais endpoints da API, simulando diferentes cenários de consulta, autenticação, gerenciamento de recursos e validação das respostas.

- **Mapeamento de Features:** Endpoints de Consulta e Endpoints de Gerenciamento.
- **Features Testadas:** Operações GET e POST, autenticação, criação de produtos, consulta de usuários e produtos, validação de Status Codes e integridade das estruturas de Request/Response.
- **Massa de Dados:** Utilização de chaves, identificadores, credenciais e dados baseados na documentação da API para validar diferentes retornos e cenários de negócio.
- **Tipos de Testes:**
  - **Testes de Funcionalidade:** Garantir que os endpoints da API estão operando e retornando os dados conforme o esperado na documentação.
  - **Testes de Integração:** Garantir que a comunicação entre o cliente e o servidor da API ocorra sem falhas de protocolo ou conexão.
  - **Testes de Contrato:** Verificar se a estrutura dos dados retornados está em conformidade com o padrão técnico estabelecido.


## Arquitetura e Estrutura

O projeto foi desenvolvido focado em testes de integração de API utilizando o ecossistema Java, priorizando a legibilidade, reutilização e validação rigorosa dos contratos:

* **Padrão de Projeto:** Java Services Pattern, organização dos testes dentro de pacotes de Services, separando as responsabilidades de cada endpoint consultado.

* **Tecnologias e Ambiente:** `Java JDK 17+` | `Git` | `Maven` | `Maven Surefire Report 3.0.0-M5` | `Exec Maven Plugin 3.1.1` | `JUnit 5.10.1` | `Rest Assured 5.3.0` | `IntelliJ IDEA`

### Componentes e Responsabilidades

- **Rest-Assured:** Utilizado como principal biblioteca para automação de API, permitindo uma sintaxe fluida (Given/When/Then) para realizar requisições HTTP e validar respostas.
- **JUnit 5:** Framework utilizado para estruturar as suítes de testes, gerenciar o ciclo de vida dos testes e fornecer relatórios de execução.
- **Java Services Pattern:** Organização dos testes dentro de pacotes de Services, separando as responsabilidades de cada endpoint consultado.
- **Asserções de Resposta:** Utilização das funcionalidades do Rest-Assured para validar Status Codes e a integridade do corpo das respostas JSON por meio de Path validation.


---

## Testes e Validações

A suíte de testes foi desenvolvida para validar os principais endpoints da API, verificando o comportamento funcional, os Status Codes, a estrutura das respostas e as regras de autenticação. Os resultados obtidos durante a execução também foram utilizados para identificar e documentar divergências entre o comportamento da API e a documentação técnica.

<details>
<summary><b>Suíte de Testes</b></summary>

A suíte contempla testes funcionais, de integração e de contrato, validando os principais endpoints da API e seus respectivos cenários de sucesso e falha.

## 1. Buscar o status da aplicação

- **Dado** que efetuo um request do método GET para o domínio `dummyjson.com/test`
- **Quando** a API retornar o Status Code e o método
- **Então** deverá conter as informações: `status: ok` & `method: GET`

## 2. Buscar usuário para autenticação

- **Dado** que efetuo um request do método GET para o domínio `dummyjson.com/users`
- **Quando** a API retornar o request solicitado
- **Então** deverá conter o status code `200` e as informações corretas do usuário

## 3. Criação de token para Autenticação

- **Dado** que efetuo um request do método POST para o domínio `dummyjson.com/auth/login` contendo `username` e `password` do usuário
- **Quando** a API retornar o request solicitado
- **Então** deverá retornar o status code `201`, contendo informações corretas do usuário e um token de autenticação funcional deve ter sido gerado.

## 4. Buscar produtos com autenticação

- **Cenário #1:** Dado que efetuo um request do método GET para `dummyjson.com/auth/products` contendo o token de autenticação correto → **Então** deverá retornar o status code `200`, contendo a lista de produtos e as informações corretas.
- **Cenário #2:** Dado que efetuo um request do método GET para `dummyjson.com/auth/products` sem informar um token de autenticação → **Então** deverá retornar o status code `403 Forbidden`, informando a mensagem `"Authentication problem"`.
- **Cenário #3:** Dado que efetuo um request do método GET para `dummyjson.com/auth/products` contendo um token inválido/expirado → **Então** deverá retornar o status code `401 Unauthorized`, informando a mensagem `"Invalid/Expired Token!"`.

## 5. Criação de produto

- **Dado** que efetuo um request do método POST para `dummyjson.com/products/add` contendo as informações do produto
- **Quando** a API retornar o request solicitado
- **Então** deverá retornar o status code `201` e as informações corretas do produto inseridas no request.

## 6. Buscar todos os produtos

- **Dado** que efetuo um request do método GET para `dummyjson.com/products`
- **Quando** a API retornar o request solicitado
- **Então** deverá retornar o status code `200` e a lista de produtos contendo as informações corretas de cada item.

## 7. Buscar apenas um produto por ID

- **Caso #1:** Dado que efetuo um request do método GET para `dummyjson.com/products/{ID}` → **Então** deverá retornar o status code `200` e apenas o produto de ID informado, contendo suas informações corretas.
- **Caso #2:** Dado que efetuo um request do método GET para `dummyjson.com/products/{ID}` informando um ID `0` ou inexistente → **Então** deverá retornar o status code `404 Not Found` seguido da mensagem `"Product with id '{id}' not found"`.

</details>

---

## Relatório de testes

Durante a automação, foram identificadas divergências entre o comportamento observado na API e os dados ou Status Codes apresentados na documentação. As inconsistências foram mapeadas na suíte de testes para facilitar sua análise e reprodução.

| Endpoint / Módulo | Suíte de Teste | Resultado esperado / Obtido |
| :--- | :--- | :--- |
| **`/test`** | `ApplicationStatusTest` | Nenhuma divergência encontrada. O response retornado está de acordo com o esperado. |
| **`/users`** | `UserAuthenticationTest` | Divergência no link da imagem esperado e no tipo sanguíneo apresentado na documentação. |
| **`/auth/login`** | `AuthenticationTest` | Status Code esperado `201`, porém a API retorna `200`. A imagem retornada também diverge da documentação e o token é gerado dinamicamente. |
| **`/auth/products`** | `AuthenticatedProductsTest` | Token estático da documentação retorna `500`, enquanto token gerado dinamicamente retorna `200`. Total esperado de produtos diverge do total retornado. |
| **`/products/add`** | `CreateProductTest` | Status Code esperado `201`, porém a API retorna `200`. Os demais parâmetros do produto são inseridos corretamente. |
| **`/products`** | `ProductsTest` | Links das imagens fornecidos na documentação divergem dos links funcionais retornados pela API. |
| **`/products/{ID}`** | `ProductByIdTest` | Links das imagens fornecidos na documentação divergem dos links funcionais retornados pela API. |

<details>
<summary><b>Detalhes: Buscar o status da aplicação</b></summary>

- **Status:** Aprovado / Sem bugs encontrados.
- **Detalhes:** O response esperado na documentação está em total acordo com o retornado na API.

</details>

<details>
<summary><b>Detalhes: Buscar usuário para autenticação</b></summary>

- **Status:** Divergências encontradas (Bugs na documentação vs. API).
- **Divergências:**
  - Link da imagem esperado: `https://robohash.org/hicveldicta.png` | Retornado: `https://robohash.org/Terry.png?set=set4`
  - Tipo sanguíneo esperado: `A−` | Retornado: `A-`

</details>

<details>
<summary><b>Detalhes: Criação de token para Autenticação</b></summary>

- **Status:** Divergências encontradas.
- **Divergências:**
  - Status code esperado: `201` | Obtido: `200`
  - Link da imagem esperado: `https://robohash.org/autquiaut.png` | Obtido: `https://robohash.org/Jeanne.png?set=set4`
  - Token: A API gera um novo token dinamicamente a cada requisição POST, invalidando o token estático sugerido na documentação.

</details>

<details>
<summary><b>Detalhes: Buscar produtos com autenticação</b></summary>

- **Status:** Divergências encontradas.
- **Divergências:**
  - O uso do token estático da documentação retorna Status Code `500` ("Invalid signature"), enquanto a geração dinâmica via POST retorna `200`.
  - Total de itens esperados: `3` | Total retornado: `30`.
  - Domínios de links de imagens divergentes em relação à documentação.
- **Testes Aprovados:** Validações de rota sem token (`403 Forbidden`) e com token inválido/expirado (`401 Unauthorized`) comportam-se perfeitamente conforme a especificação.

</details>

<details>
<summary><b>Detalhes: Criação de produto</b></summary>

- **Status:** Divergências encontradas.
- **Divergências:**
  - Status code esperado: `201` | Retornado: `200`.
  - Demais parâmetros do produto inseridos corretamente.

</details>

<details>
<summary><b>Detalhes: Buscar todos os produtos e Buscar por ID</b></summary>

- **Status:** Divergências encontradas.
- **Divergências:**
  - Os links das imagens fornecidos na documentação divergem dos links funcionais retornados pela API.

</details>

---

# Passos para Configurar e Reproduzir o Projeto

Siga o guia abaixo para clonar, configurar o ambiente e executar a suíte de testes automatizados em sua máquina local.

---

## Pré-requisitos

Certifique-se de possuir as seguintes ferramentas instaladas em seu ambiente:

- [Git](https://git-scm.com/)
- [Java JDK](https://adoptium.net/) — versão 17 ou superior recomendada
- [Maven](https://maven.apache.org/)
- Uma IDE Java de sua preferência, como [IntelliJ IDEA](https://www.jetbrains.com/idea/)

---

## Obtendo o Código do Projeto

Você pode obter os arquivos do projeto de duas formas.

### Opção A: Clonando via Git (Recomendado)

Abra o terminal e execute o comando abaixo para clonar o repositório:

```bash
git clone https://github.com/FabioGVL/SicrediAPI.git
```

Em seguida, navegue para dentro da pasta do projeto:

```bash
cd SicrediAPI
```

### Opção B: Baixando via ZIP

1. Acesse a página do repositório no GitHub.
2. Clique no botão verde **Code**.
3. Selecione **Download ZIP**.
4. Extraia o conteúdo do arquivo compactado em uma pasta no seu computador.
5. Abra sua IDE Java.
6. Acesse a opção para abrir um projeto existente e selecione a pasta descompactada.

---

## Instalando as Dependências

Com o terminal aberto na raiz do projeto, execute o comando abaixo para instalar as dependências e executar o ciclo de testes:

```bash
mvn clean test
```

---

## Executando os Testes

O projeto utiliza o Maven para execução da suíte automatizada e geração dos relatórios.

### Execução Padrão

Executa toda a suíte de testes e realiza a limpeza do projeto antes da execução:

```bash
mvn clean test
```

### Gerando o Relatório Maven Surefire

Para gerar o relatório completo de execução dos testes:

```bash
mvn surefire-report:report
```

### Executando um Teste Específico

Para executar uma classe de teste específica:

```bash
mvn test -Dtest=ApplicationStatusTest
```


---

# CI/CD e Relatórios de Testes

Para gerar e visualizar o relatório completo de execução via GitHub Actions:

1. Acesse o repositório no GitHub: `https://github.com/FabioGVL/SicrediAPI`.
2. No menu superior, clique em **Actions**.
3. Selecione o workflow desejado e verifique se a label `SUMMARY` está ativa.
4. No rodapé da página de execução concluída, faça o download do arquivo compactado no campo **ARTIFACTS**.
5. Extraia o arquivo `.zip` e abra o relatório HTML `surefire-report.html` em seu navegador.

---

# Resumo dos Comandos

| Objetivo | Comando |
|---|---|
| Instalar dependências / Limpar projeto | `mvn clean test` |
| Gerar relatório Surefire Report | `mvn surefire-report:report` |
| Executar teste específico via Maven | `mvn test -Dtest=ApplicationStatusTest` |
