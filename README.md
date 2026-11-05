# QUALITYCORE v0.28.0

Local MVP for API testing and synthetic monitoring, built with Java 21, Spring Boot, REST Assured, Spring Data JPA, H2, HTML, CSS and JavaScript.

## What's new in v0.10

- Persisted Collections stored in the database.
- Collections belong to a Project and have a unique name within that Project.
- Select API Tests from the same Project when creating a Collection.
- Edit and persist the ordered steps; add, remove and move steps up/down.
- Persist a Collection failure policy (`STOP` or `CONTINUE`).
- Execute a saved Collection using its persisted step order and failure policy.
- Existing API test configuration, assertions, variable extraction and Collection execution report remain available.

## Requirements

- Java 21
- Maven 3.9 or compatible

## Run

```bash
mvn clean test
mvn spring-boot:run
```

Open `http://localhost:8080`.

The application stores local data in `./data/qasentinel` using H2. Back up this directory before deleting or replacing the local database.

## Notes and limitations

- This is a local-development MVP, not a production-hardened service.
- Do not expose it publicly until authentication, SSRF defenses, secret handling, request limits, and production database operations are implemented.
- The `STOP`/`CONTINUE` policy is saved per Collection. Explicit dependency graph and skip-dependent policy are planned for a later increment.
- Collection step order is persisted. Reordering is currently done using the up/down controls rather than drag-and-drop.


## QUALITYCORE v0.28.0 — Cypress: descoberta dos testes

- **Framework:** o cadastro aceita **Playwright** ou **Cypress**. A linguagem continua JavaScript ou TypeScript.
- **Descoberta do Cypress:** lê o `specPattern` do `cypress.config` (ou `cypress/e2e/**/*.cy.{js,jsx,ts,tsx}`), lista os arquivos e os testes `it(...)`. Não instala o Cypress e não baixa o binário, então a listagem não depende do servidor de download.
- **Classificação:** `cy.visit`, `cy.get` e afins contam como funcional; `cy.request` e `cy.intercept` como API.
- **Regras:** `it.skip` e `xit` não entram na contagem. Testes criados em laço, sem título fixo no código, não aparecem.
- **Execução do Cypress:** ainda não disponível. Ao executar, a aplicação informa isso com clareza.
- **Validação:** testes Java com um projeto montado na estrutura do Cypress (191 testes passando). Não validado: a listagem em um projeto Cypress real, porque o binário não está acessível daqui.

## QUALITYCORE v0.27.1 — Exploração de repositórios privados com o token salvo

- **Causa:** a exploração do repositório usava só o token digitado no formulário. Ao usar **Alterar**, o campo do token fica vazio (o token salvo não é exibido), então a exploração saía sem credencial. Repositórios privados pediam usuário e senha, e a aplicação não podia perguntar.
- **Correção:** ao explorar ou listar as branches de um repositório salvo, sem token digitado, a aplicação usa o token guardado.
- **Mensagem:** erros de autenticação e de repositório não encontrado agora explicam o que fazer.
- Testes Java: 186 passando. Testes de interface: todas as suítes passando.

## QUALITYCORE v0.27.0 — Capturas de tela em falhas (etapa 1)

- **Opção por repositório:** *Capturas de tela*: **Só em falhas** (padrão, recomendado), **Em todos os testes** ou **Nenhuma**. Testes de API não geram capturas.
- **Execução:** a opção vira o argumento `--screenshot` do Playwright. As capturas são copiadas para `data/functional/evidence/run-<id>/` logo após a execução, porque a próxima atualização do repositório limpa a pasta clonada.
- **Vínculo:** cada captura fica ligada ao teste, à linha e ao navegador, no resultado da execução.
- **Execuções:** cada teste com captura mostra uma miniatura. Ao clicar, a imagem abre em uma nova aba. O servidor só entrega arquivos PNG guardados pela aplicação.
- **Validação:** testes Java de leitura, cópia, recusa de capturas ausentes e restrição de nomes. Testes de interface de miniaturas e carregamento autenticado. Não validada aqui a geração real de captura, porque este ambiente não tem navegadores.

### Ainda não implementado (próximas etapas)
2. Vídeo e trace, guardados pelo mesmo caminho das capturas.
3. Monitores com Functional Testing e escolha de arquivos.
4. Cypress (JavaScript e TypeScript).
5. Relatório da execução em HTML, com as capturas, e depois em PDF.
6. JMeter ligado à execução.
7. RestAssured (Java) e Robot Framework (Python).

## QUALITYCORE v0.26.0 — Arquivos com falha no histórico e dica para navegadores

- **Histórico de execuções:** cada execução tem o botão **Ver arquivos**. Ele lista os arquivos que tiveram falha naquela execução, com os testes que falharam, o navegador e o primeiro erro.
- **Navegador que não abre** (por exemplo, `Failed to resize window` no WebKit): a mensagem da execução indica a causa provável (bibliotecas do sistema) e o comando `npx playwright install-deps`, que exige permissão de administrador.

### Pendências desta rodada (ainda não implementadas)
- Login e senha além do token: recomendo manter o token para GitHub e GitLab, porque ambos deixaram de aceitar senha em HTTPS. Usuário e senha só fariam sentido para um GitLab próprio que aceite autenticação básica.
- Frameworks adicionais (Java com RestAssured, Python com Robot Framework e Cypress com JavaScript/TypeScript): cada um precisa de uma leitura própria dos testes e de um relatório próprio.
- Evidências (capturas, vídeo e trace) e relatório em HTML ou PDF.
- Monitores com testes funcionais, com seleção de arquivos.
- Motor JMeter ligado à execução.

## QUALITYCORE v0.25.2 — Opção JMeter visível para pré-visualização

- A opção **JMeter** no campo Motor de carga agora pode ser selecionada, para avaliar como a tela fica.
- Ao tentar executar com JMeter, a tela informa que o motor está em validação e não envia nada. O servidor continua recusando a execução com a mesma regra.
- A execução de JMeter continua indisponível até a validação na máquina de teste.
- Testes de interface: todas as suítes de Performance passando.

## QUALITYCORE v0.25.1 — Categoria não parece falha

- Na tela **Ver testes**, a coluna **Categoria** usava o mesmo selo vermelho **FAIL** que indica falha de execução. Os testes funcionais apareciam como falhados, mesmo sem terem sido executados.
- Agora a categoria aparece como uma etiqueta neutra (API, FUNCIONAL, MISTO ou INDEFINIDO).
- A informação de aprovação ou falha continua apenas na tela **Execuções**, que mostra o resultado real de cada execução.
- Testes de interface: todas as suítes passando.

## QUALITYCORE v0.25.0 — Repositórios de API em API Tests, seleção de testes e instalador do JMeter

### Separação de API Tests e Functional Testing
- **API Tests** tem a seção **Repositórios de testes de API**: cadastro, branches, exploração de pastas, descoberta, execuções e histórico, com a mesma implementação usada no Functional Testing.
- **Functional Testing** lista só repositórios de navegador (Funcional e E2E). O tipo API não é oferecido ali.
- O **pa2** (repositório de API) aparece agora em API Tests. Nada foi recadastrado: o cadastro, o histórico de execuções e o tipo continuam os mesmos.

### Seleção de testes de API
- Nos repositórios de API, a descoberta mostra uma caixa por teste. **Salvar seleção** grava os testes marcados.
- Com seleção, a execução roda só os testes marcados, pelo filtro `--grep` do Playwright. Sem seleção, a pasta inteira roda.
- Os títulos são escapados antes de virar filtro, para que caracteres como parênteses não quebrem a busca.
- Limitação: o filtro é pelo título. Dois testes com o mesmo título em arquivos diferentes são selecionados juntos.

### Instalador do JMeter (código pronto, ainda não ligado à execução)
- Localiza o JMeter pela configuração `qa.jmeter.path`, ou por uma instalação anterior, ou baixa a versão `qa.jmeter.version` (padrão 5.6.3) do arquivo oficial da Apache.
- O arquivo só é usado se o SHA-512 bater com o publicado pela Apache para a mesma versão, a mesma regra do k6. Um download que não bate é descartado.
- A extração recusa qualquer entrada que tentaria gravar fora da pasta de instalação.
- Não validado aqui: o download, porque este ambiente não acessa o site da Apache. Teste na sua máquina.

### Ainda não existe
- A execução de JMeter. O instalador ainda não está ligado ao motor de performance, e o JMeter continua desabilitado no campo Motor.

### Validação
- Testes Java: 178 passando, incluindo o SHA-512 contra um valor conhecido, a recusa de entradas que saem da pasta, o escape dos títulos e a montagem do filtro de seleção.
- Testes de interface: todas as suítes passando, incluindo a seção de API Tests, a seleção de testes e a ausência do tipo API no Functional Testing.
- Um teste de estabilidade corrigido: a verificação de dados adulterados do cofre de segredos dependia de um caractere de preenchimento, e agora altera um byte real.

## QUALITYCORE v0.24.1 — Campo Motor de carga na tela de Performance

- **Campo Motor de carga** no formulário de Performance, antes do tipo de alvo. **k6** vem selecionado. **JMeter** aparece desabilitado, com a indicação "em validação".
- **Servidor:** cada execução grava o motor escolhido. Se o motor for JMeter, a criação é recusada com a mensagem de validação. Um motor inválido também é recusado.
- **Lista de execuções:** nova coluna **Motor**. Execuções criadas antes desta versão aparecem como k6.
- A liberação do JMeter depende da validação da execução na sua máquina, como descrito na versão 0.24.0.
- Testes Java: 170 passando. Testes de interface: todas as suítes passando.

## QUALITYCORE v0.24.0 — JMeter (base): plano de teste e leitura dos resultados

Esta versão traz a base do motor JMeter, ainda sem tela. O motor só aparece na interface depois da validação com um JMeter real.

- **Gerador de plano (`JmeterPlanBuilder`):** monta um `.jmx` com um grupo de threads (usuários, rampa e duração) e um sampler HTTP para o alvo. Limites: até 50 usuários e 600 segundos, o mesmo teto do k6. URLs inválidas e métodos não permitidos são recusados antes de gerar o plano.
- **Leitor do JTL (`JtlAggregator`):** lê o CSV de resultados que o JMeter escreve durante a execução, localizando as colunas pelo cabeçalho. Calcula totais, p50, p90, p95, p99, vazão, taxa de erro e, por segundo, requisições, erros e grupos de status (2xx, 3xx, 4xx, 5xx).
- Testes Java: 166 passando (inclui o plano gerado, validado como XML, e o leitor com quebras de linha entre aspas).

### O que não foi validado
- O JMeter real não rodou aqui. A versão disponível no sandbox (JMeter 2.13, pelo repositório do Ubuntu) não aceita o XStream instalado junto com ela. Por isso o plano e o JTL foram validados por XML bem formado e por testes com arquivos no formato do JMeter, mas não por uma execução completa.
- Para validar na sua máquina: instale o JMeter 5.6 ou superior, gere um plano com o `JmeterPlanBuilder` e execute `jmeter -n -t plano.jmx -l resultados.jtl`. Depois, leia o `resultados.jtl` com o `JtlAggregator`.

### Ainda não existe
- A escolha do motor (k6 ou JMeter) na tela de Performance.
- A parada e o relatório do JMeter. O JMeter não tem a API de parada que o k6 tem, e o relatório HTML precisa ser avaliado.

## QUALITYCORE v0.23.1 — Navegadores do Playwright instalados automaticamente

- **Causa:** a execução guardava os navegadores dentro de cada clone do repositório (`node_modules/.../.local-browsers`). Por isso cada repositório precisava de uma instalação separada, e ninguém instalava nada automaticamente.
- **Correção:** os navegadores ficam no cache compartilhado do Playwright (`~/.cache/ms-playwright` no Linux e macOS, `%USERPROFILE%\AppData\Local\ms-playwright` no Windows). Uma instalação serve a todos os repositórios da máquina.
- **Na inicialização** (`mvn spring-boot:run`): em segundo plano, a aplicação instala os navegadores da versão do Playwright de cada repositório já clonado. Ela não espera a instalação para subir.
- **Antes de cada execução de interface:** a execução verifica os navegadores e instala os que faltarem. Testes de API não instalam navegador.
- Com o cache padrão, o `npx playwright install` que você executava manualmente deixa de ser necessário.
- Testes Java: 158 passando. A instalação em si depende de acesso aos servidores do Playwright, e não foi validada neste ambiente.

## QUALITYCORE v0.23.0 — Opções de execução e mensagens de navegador

- **Navegador visível (headed):** nova opção no cadastro do repositório. A execução abre a janela do navegador, um teste por vez. Só mostra a janela quando o QUALITYCORE roda na própria máquina de quem executa; em um servidor sem tela, a janela não aparece.
- **Testes de API:** a execução usa um único projeto configurado (não roda o mesmo teste em três navegadores). Com isso, o teste do seu repositório de API passou de 18 para 6 execuções, e de 43 para 17 segundos.
- **Coluna Navegador:** não aparece nos resultados de repositórios de API.
- **Mensagem de navegador:** o banner do Playwright não aparece mais na mensagem da execução. Ela traz a primeira linha do erro e a contagem de ocorrências.
- Testes Java: 158 passando. Testes de interface: todas as suítes passando.

### Decisões pendentes (não implementadas)
- **Testes de API vindos do GitHub/GitLab dentro de API Tests:** recomendado. Ver a discussão da versão.
- **Tipo de alvo "Functional Testing" em Performance e Monitors:** depende de definir o que significa medir desempenho de um teste funcional.
- **Visualização ao vivo da execução:** a opção headed resolve quando a execução é local. Para acompanhar de outra máquina, seria preciso transmitir capturas da tela durante a execução.

## QUALITYCORE v0.22.3 — Leitura do testDir e checagem da pasta

- **Causa:** a checagem "a pasta está fora do testDir" dependia de uma listagem completa do projeto. Quando essa listagem falhava, a checagem era pulada silenciosamente, e a mensagem não aparecia.
- **Correção:** o `testDir` é lido primeiro do relatório do Playwright (sem o mecanismo de recuperação) e, se não estiver lá, diretamente do `playwright.config`.
- **Configuração para testes na raiz do repositório:** pasta do projeto vazia, pasta dos testes `tests/ui` ou `tests/api`.
- Testes Java: 156 passando. Testes de interface: todas as suítes passando.

## QUALITYCORE v0.22.2 — Layout do formulário, pasta fora do testDir e navegadores

- **Layout:** o campo de branch tem linha própria. A lista de branches e os botões **Listar branches** e **Explorar pastas** quebram de linha quando não há espaço, e nenhum botão fica escondido.
- **Tipo de teste:** rótulos curtos (Funcional, E2E, API).
- **Pasta fora do testDir:** o Playwright só encontra testes dentro do `testDir` do `playwright.config`. Se a pasta dos testes estiver fora dele (por exemplo `Playwright/projeto02/tests/ui` com a pasta do projeto vazia), a descoberta e a execução param com a mensagem que indica a pasta do projeto e a pasta dos testes corretas, em vez de "No tests found".
- **Navegadores:** quando um teste de interface falha porque o navegador não está instalado, a mensagem da execução indica `npx playwright install`.
- Testes Java: 155 passando. Testes de interface: todas as suítes passando.

## QUALITYCORE v0.22.1 — Arquivo quebrado não zera a pasta de testes

- **Causa:** o Playwright carrega a pasta inteira de uma vez. Um único arquivo que não importa suas dependências (por exemplo, `tests/users.spec.js` com `../../clients/ApiClient`) fazia a listagem de **toda a pasta** falhar, com 0 testes e a mensagem "No tests found".
- **Correção:** quando um arquivo não carrega, o QUALITYCORE identifica o arquivo pelo erro, executa os **demais** explicitamente, e mostra o arquivo deixado de fora com o motivo, na descoberta e na execução.
- **Validação:** no repositório real, a pasta `tests` agora lista 6 arquivos e 16 testes (7 de API e 9 funcionais). O `tests/users.spec.js` aparece separado, com o erro `Cannot find module '../../clients/ApiClient'`.
- Testes Java: 154 passando. Testes de interface: todas as suítes passando.

## QUALITYCORE v0.22.0 — Execução dos testes, resultados e alteração de repositórios

### Execução
- Botão **▶ Executar** em cada repositório. A execução roda em segundo plano: a tela mostra o andamento e atualiza sozinha enquanto estiver em andamento.
- Cada execução guarda: testes únicos, execuções (um teste roda uma vez por navegador configurado), aprovadas, falhas, ignoradas, instáveis, duração e commit.
- O resultado de cada teste fica disponível: arquivo, linha, navegador, resultado, duração e a mensagem de erro (sem códigos de cor do terminal).
- Histórico de execuções com gráfico de aprovadas e falhas por execução.
- Se a pasta dos testes não existir no projeto, a execução para antes de rodar e mostra as pastas que existem.
- Quando há falhas, a mensagem da execução traz o erro mais comum, para diferenciar falhas do código das falhas de ambiente (por exemplo, sem acesso à internet).

### Tipo de teste
- As opções são **Funcional (interface)**, **E2E (ponta a ponta)** e **API (endpoints)**.

### Branch
- O campo de branch é uma lista com todas as branches do repositório. A lista é carregada ao digitar o endereço ou ao clicar em **Listar branches**.

### Alterar repositório
- O botão **Alterar** carrega no formulário todos os dados salvos. O token não é mostrado: se o campo ficar vazio, o token atual é mantido.

### Limitações conhecidas
- Nomes de branch com barra (por exemplo `feature/login`) não são lidos de um endereço de pasta. Escolha a branch na lista.
- A execução roda no próprio servidor, como a descoberta. A execução isolada em Docker (fase 3 completa) ainda não existe, então use somente repositórios de confiança.
- O painel de monitoria dos testes funcionais ainda não existe.

### Validação
- Execução de ponta a ponta com o repositório real `antoniogmartins/Treinamento`, pasta `tests/api`: 18 execuções, 6 testes únicos, todas com falha por falta de acesso à internet no ambiente de desenvolvimento (`getaddrinfo ENOTFOUND`). A mensagem da execução identificou essa causa.
- Pasta inexistente: a execução para com a mensagem que lista as pastas disponíveis.
- Testes Java: 153 passando. Testes de interface: todas as suítes passando.

## QUALITYCORE v0.21.1 — Correção da descoberta com pasta do projeto

- **"Pasta do projeto fora do repositório":** a checagem de caminhos falhava com o diretório de dados padrão (`./data/functional`), porque o caminho absoluto mantinha um `.` que a normalização removia. Isso quebrava a descoberta de qualquer repositório. Os caminhos agora são normalizados antes da comparação.
- **Mensagem quando não há `package.json`:** se a pasta do projeto estiver vazia e a raiz não tiver projeto Playwright, a mensagem lista os projetos Playwright encontrados no repositório.
- **Classificação API/funcional:** o Playwright informa cada arquivo relativo ao `testDir` da configuração, e não à pasta filtrada. O código de cada teste agora é localizado pelo caminho dentro do projeto, preferindo a cópia dentro da pasta de testes escolhida. Antes, todos os testes saíam como INDEFINIDO.

### Validação com o repositório real `antoniogmartins/Treinamento`, branch `main`
- Pasta do projeto vazia (raiz) e pasta dos testes `tests/api`: DONE, 6 testes, todos API.
- Pasta do projeto `Playwright/projeto02` e pasta dos testes `tests/api`: DONE, 6 testes, todos API.
- Pasta do projeto vazia (raiz) e pasta dos testes `tests/ui`: DONE, 9 testes, todos FUNCIONAL.
- Testes Java: 150 passando.

## QUALITYCORE v0.21.0 — Testes de API e funcionais, exploração de pastas e mensagens de erro

### Classificação: API ou funcional, por teste
- Cada teste é classificado pelo código do próprio teste (do início dele até o próximo), não pelo nome do arquivo:
  - **API:** usa `request`, `apiRequest`, `apiClient`, `newContext`, ou inspeciona `response.status()/json()`.
  - **FUNCIONAL:** usa `page`, `browser`, `locator`, `getBy*` ou `goto`.
  - **MISTO:** tem sinais dos dois (ex.: fluxo de interface que também chama a API).
  - **INDEFINIDO:** sem sinais claros.
- A descoberta mostra os totais por categoria e a lista de testes com linha e categoria.
- É uma heurística baseada em sinais de código: um teste sem nenhum desses padrões fica como INDEFINIDO, e não é possível garantir a classificação em todos os casos.

### Exploração do repositório (botão "Explorar pastas")
- Cole a raiz do repositório (ou um endereço de pasta). O QUALITYCORE clona a branch, mapeia todos os projetos Node (`package.json`), indica quais usam Playwright e lista as pastas que contêm testes, com a quantidade de arquivos em cada uma.
- Escolha o projeto e a pasta dos testes na lista; a tela preenche os campos do cadastro.
- Pastas `node_modules`, `.git` e similares são ignoradas.

### Mensagens de erro
- O Spring Boot oculta a mensagem das exceções por padrão, por isso a tela mostrava apenas "HTTP 404". Agora `server.error.include-message=always` envia a mensagem.
- Na lista de repositórios, uma descoberta com falha mostra o motivo em texto abaixo do status.

### Validação
- Exploração e categorias rodadas no repositório real `antoniogmartins/Treinamento`:
  - `Playwright/projeto01/qa-playwright-js` e `Playwright/projeto02` identificados como projetos Playwright; a pasta Python foi ignorada.
  - No `projeto02`: 2 testes de API (`api/requisicoes.spec.js`, `api/users.spec.js`) e 9 funcionais (`ui/checkout`, `ui/login`, `ui/produto`).
- Testes Java: 145 passando. Testes de interface: todas as suítes passando.

### Ainda não validado
- A mensagem completa de erro depende do `server.error.include-message`. Confirme na tela "Ver testes" após uma descoberta com falha.
- A execução dos testes ainda não existe (fase 3). Testes de API com o fixture `request` não precisam de navegador, e o worker deve tratar esse caso.

## QUALITYCORE v0.20.2 — Endereços de pasta e pasta do projeto

- **Endereço de pasta:** agora é aceito o endereço copiado do navegador, como `https://github.com/owner/repo/tree/main/Playwright/projeto02` (GitHub) ou `https://gitlab.com/grupo/projeto/-/tree/main/pasta` (GitLab). A branch e a pasta são lidas do próprio endereço.
- **Pasta do projeto:** novo campo que indica onde fica o `package.json` dentro do repositório. A descoberta roda nessa pasta, e o diretório dos testes é relativo a ela. Ao colar um endereço de pasta, o formulário preenche branch e pasta automaticamente.
- **Endereços de arquivo** (`/blob/...`) são recusados, com a indicação de usar a pasta.
- **Limitação:** nomes de branch com barra (por exemplo `feature/login`) não são lidos do endereço. Informe-os no campo Branch.

### Validação
- Pipeline completo no repositório real `antoniogmartins/Treinamento`, pasta `Playwright/projeto02`: 5 arquivos, 11 testes, navegadores chromium, firefox e webkit.
- Testes Java: 138 passando.

## QUALITYCORE v0.20.1 — Correção de inicialização do contexto Spring

- **Correção:** `SecretBox` (criptografia de tokens, criada na v0.20.0) tinha dois construtores sem `@Autowired`, o mesmo erro corrigido no `JwtService` na v0.19.1. A aplicação não subia, e o `HealthControllerTest` falhava.
- **Prevenção:** o teste `BeanWiringTest` agora varre todos os beans do projeto (classes com `@Component`, `@Service`, `@Configuration` ou `@RestController`) e exige um construtor marcado sempre que houver mais de um. Verificado: sem a anotação, o teste falha.

### Validação
- Suíte Java: 134 testes passando (sem o `HealthControllerTest`, que sobe o Spring completo).
- Pendente: `mvn clean test` com o `HealthControllerTest` no seu ambiente.

## QUALITYCORE v0.20.0 — Functional Testing: repositórios e descoberta (fases 1 e 2)

Novo módulo para testes funcionais automatizados. Esta versão entrega o cadastro de repositórios e a descoberta dos testes. A execução dos testes, os resultados e os dashboards são as fases 3 a 5.

### O que está pronto

- **Cadastro de repositório** (menu **Functional**): provedor (GitHub ou GitLab), endereço https, branch (com listagem das branches existentes), linguagem (JavaScript ou TypeScript), tipo de teste e diretório dos testes.
- **Repositórios públicos e privados:** para privados, informe um token de acesso pessoal. O token é criptografado (AES-256-GCM) antes de ir para o banco e nunca volta para a tela.
- **Acesso restrito a administradores:** todas as rotas `/api/functional/**` exigem perfil ADMIN. Usuários comuns recebem 403.
- **Descoberta dos testes:** o código é clonado (ou atualizado com `fetch`) na branch escolhida, as dependências são instaladas com `npm ci --ignore-scripts` e os testes são listados pelo próprio Playwright (`playwright test --list --reporter=json`). Assim valem o `testDir`, o `testMatch` e os projetos do `playwright.config`, não o nome dos arquivos.
- **Resultado da descoberta:** arquivos, quantidade de testes, tags, navegadores configurados, commit analisado e erros do Playwright.

### Limitação importante: execução no servidor

A descoberta **executa código do repositório no servidor do QUALITYCORE**, porque o `playwright.config.js` é JavaScript. Por isso:

- Use somente repositórios de confiança até a fase 3, quando a descoberta e a execução passam para o worker em Docker isolado.
- A restrição a administradores é a proteção desta etapa.

### Requisitos no servidor

- `git` instalado (acessível no PATH).
- Node.js e npm instalados (para a descoberta). A instalação do Playwright e dos navegadores é tarefa do worker, na fase 3.

### Armazenamento

- Código dos repositórios: `data/functional/repos/<id>/`.
- Chave que cifra os tokens: `qa.secrets.key` (base64 de 32 bytes). Se vazia, é criada em `data/functional/secrets.key`. **Faça backup dessa chave:** sem ela, os tokens salvos não podem ser lidos.

### Pontos ainda não validados

- **GitLab:** a leitura do endereço e a autenticação estão implementadas, mas não testei com um GitLab real.
- **GitHub privado:** a autenticação com token é a mesma usada para repositórios públicos, mas não testei com um repositório privado real.
- **Windows:** a chamada de `npm` e `npx` usa os executáveis `.cmd`. Não testei nesse sistema.

### Validação

- Testes Java: 134 passando. Cobrem leitura de URL (incluindo recusa de credenciais, parâmetros e `..`), criptografia (chave errada e valor alterado), papéis, o token fora da serialização, o parser da descoberta com a saída real do Playwright, e o Git com um repositório local (clonagem, listagem de branches e atualização após novo commit).
- **Descoberta real:** o fluxo completo rodou neste ambiente com um projeto Playwright de exemplo: Git, npm e `playwright --list`. O resultado foi 2 arquivos, 3 testes, navegadores `chromium` e `firefox`, e as tags `smoke` e `regression`.
- Testes de interface: a tela para administrador e para usuário comum.

### Arquivos novos

- `functional/RepositoryUrl.java`, `functional/GitClient.java`, `functional/PlaywrightDiscovery.java`.
- `service/CodeRepositoryService.java`, `api/CodeRepositoryController.java`.
- `model/CodeRepository.java`, `repo/CodeRepositoryRepository.java`.
- `security/SecretBox.java`.

### Arquivos alterados

- `security/UserAccount.java` (papel ADMIN ou USER), `security/AuthService.java` (verificação de administrador), `api/AuthController.java` (papel em `/auth/me`), `security/JwtService.java`.
- `static/app.js`, `static/index.html`, `static/styles.css`, `application.properties`, `pom.xml`, `LICENSE`, `README.md`.

## QUALITYCORE v0.19.2 — Correções de Contracts e do relatório de performance

- **Excluir contrato:** o erro HTTP 500 era causado por uma exclusão derivada do Spring Data, executada fora de transação. A exclusão agora remove o histórico pelas entidades carregadas.
- **Editar contrato:** a lista de Contracts tem o botão **Editar**, que altera o nome e o JSON de um contrato salvo (`PUT /api/contracts/{id}`). O histórico de verificações é mantido. Para trocar as operações, edite o JSON e verifique de novo.
- **additionalProperties:** quando o schema declara `"additionalProperties": false`, campos não previstos reprovam a verificação, com a indicação do campo.
- **Relatório de performance:** o relatório é buscado com o token da sessão e aberto como conteúdo local. Antes, a nova aba abria a API sem o token, e aparecia "Sessão expirada" mesmo com login ativo.

### Validação
- Testes Java: 122 passando, incluindo exclusão, edição, rejeição de contrato inválido e `additionalProperties`.
- Testes de interface: edição de contrato, relatório com token e as suítes anteriores.
- Não executado: verificação contra o JSONPlaceholder (o ambiente de desenvolvimento não tem acesso a essa API).

## QUALITYCORE v0.19.1 — Correção de inicialização

- **Correção:** a aplicação não subia com o contexto do Spring. A classe `JwtService` tinha dois construtores e nenhum estava marcado com `@Autowired`, então o Spring não sabia qual usar e falhava ao criar o bean. Agora o construtor de produção está marcado.
- Teste de regressão novo (`BeanWiringTest`) garante que um bean com vários construtores sempre declara qual deve ser usado.

## QUALITYCORE v0.19.0 — Autenticação com JWT, testes de contrato e ajustes do dashboard

### Autenticação (JWT)

- Toda a API (`/api/**`) exige um token. As exceções são o status de configuração, a criação da conta inicial, o login e a verificação de saúde.
- **Primeiro acesso:** a tela pede a criação do administrador. Depois disso, a configuração inicial não pode ser repetida.
- **Login:** usuário e senha. A mensagem de erro é a mesma para usuário inexistente e senha errada, para não revelar quais contas existem.
- **Senhas:** guardadas com PBKDF2-HMAC-SHA256 (210 mil iterações e sal aleatório por senha). Nunca em texto puro.
- **Tokens:** JWT HS256 com validade configurável (`qa.auth.token-hours`, padrão 8 horas). São rejeitados tokens com outra assinatura, adulterados, expirados ou com algoritmo diferente de HS256 (inclusive `alg: none`).
- **Sessão expirada:** a tela volta para o login automaticamente, sem perder a tela em que o usuário estava.
- **Contas:** a aba **Conta** em Settings permite sair e criar novos usuários. Todos os usuários têm os mesmos poderes nesta versão (não há perfis).

**Configuração antes de uso real:** defina `qa.auth.secret` com um valor fixo de 32 caracteres ou mais. Sem isso, a chave é aleatória e as sessões são perdidas a cada reinício.

### Testes de contrato (novo menu "Contracts")

Um contrato descreve as operações de uma API: método, caminho, parâmetros, corpo da requisição e respostas com seus schemas. Há duas formas de cadastro, que gravam o mesmo formato:

1. **Manual:** você escreve o contrato em JSON na tela (há um botão para preencher um modelo).
2. **Importação OpenAPI 3:** você cola a especificação em JSON, ou escolhe um arquivo `.json`. Referências internas (`$ref` para `#/components/...`) são resolvidas. Referências externas são recusadas, porque o arquivo precisa ser autocontido.

**YAML não é aceito.** Converta a especificação para JSON antes de importar.

#### O que a verificação valida, por operação

| Verificação | O que confirma |
|---|---|
| Exemplo de requisição respeita o schema | O corpo de exemplo do contrato cumpre o schema declarado (tipos, obrigatórios, formatos). |
| Status HTTP previsto pelo contrato | A API responde com um dos status declarados (ou 2xx, se nenhum for declarado). |
| Resposta respeita o schema declarado | O corpo da resposta cumpre o schema do status recebido. |
| Recusa requisição sem o campo obrigatório (4xx) | Para **cada** campo obrigatório do corpo, a API recusa a requisição sem ele. |
| Recusa requisição sem o parâmetro obrigatório (4xx) | A API recusa uma requisição sem o parâmetro de query obrigatório. |

Quando o contrato não traz exemplos, os valores são gerados a partir dos schemas (apenas os campos obrigatórios).

#### Formato do contrato

```json
{"title":"Users API","operations":[
  {"method":"GET","path":"/users/{id}",
   "parameters":[{"name":"id","in":"path","required":true,"schema":{"type":"integer","minimum":1}}],
   "responses":{"200":{"schema":{"type":"object","required":["id","name"],
     "properties":{"id":{"type":"integer"},"name":{"type":"string"}}}}}},
  {"method":"POST","path":"/users",
   "requestBody":{"required":true,"schema":{"type":"object","required":["name","email"],
     "properties":{"name":{"type":"string","minLength":2},"email":{"type":"string","format":"email"}}}},
   "responses":{"201":{"schema":{"type":"object","required":["id"],"properties":{"id":{"type":"integer"}}}}}}
]}
```

Schemas suportam: `type`, `required`, `properties`, `items`, `enum`, `minLength`, `maxLength`, `minimum`, `maximum`, `pattern` e os formatos `email`, `uuid`, `date` e `date-time`.

#### Resultados e gráficos

- Cada verificação fica no histórico. A tela mostra o resultado da última execução, a cobertura (operações aprovadas na última execução, em %) e um gráfico de barras empilhadas com aprovadas e reprovadas ao longo das verificações.
- Os contratos ficam com o detalhe de cada verificação, com o motivo de cada falha.

**Limitações:** não há matriz de dependências entre consumidores e provedores, nem cobertura por endpoint em relação ao tráfego de produção. Esses gráficos exigem coletar o tráfego real e um cadastro de consumidores, que ainda não existem.

### Dashboard: detalhe por API do monitor

- Ao clicar em um monitor da legenda, o detalhe das APIs dele abre e **permanece aberto** durante as atualizações automáticas.
- Clicar de novo no mesmo monitor fecha o detalhe.

### Monitors: APIs de cada monitor

- Os cards de monitores de Collection têm o botão **Ver APIs**, que abre e fecha o painel com a latência de cada API. O painel não abre sozinho.

### Correção

- O carregamento inicial da aplicação não pode depender de uma constante declarada no fim do arquivo. Isso causaria um erro ao abrir a tela, e foi corrigido antes da entrega.

### Arquivos alterados

- Back-end (novos): pacote `security` (`PasswordHasher`, `JwtService`, `UserAccount`, `AuthService`, `AuthInterceptor`, `WebSecurityConfig`), `api/AuthController`, `api/ContractController`, `model/ApiContract`, `model/ContractRun`, `repo/UserAccountRepository`, `repo/ApiContractRepository`, `repo/ContractRunRepository`, `service/JsonSchemaValidator`, `service/ExampleGenerator`, `service/OpenApiImporter`, `service/ContractVerifier`, `service/ContractService`.
- Front-end: `static/app.js`, `static/index.html`, `static/styles.css`.
- Configuração: `application.properties`, `pom.xml`, `LICENSE`, `README.md`.

### Validação

- Testes Java: 115 passando, incluindo JWT (assinatura, adulteração, `alg: none`, expiração), senhas, regras de contas, validador de schema, gerador de exemplos, importador OpenAPI e o verificador contra um servidor HTTP real que cumpre o contrato e outro que não cumpre.
- Testes de interface: login e expiração de sessão, cadastro manual, importação, verificação e resultado, botão Ver APIs, detalhe persistente no dashboard, aba Conta.

### Limitações desta versão

- O interceptador de autenticação (que protege as rotas) é compilado, mas não tem teste de execução aqui, porque depende do Spring Boot real. Valide com `mvn clean test` e uma chamada sem token a `/api/tests`, que deve devolver 401.
- O token fica guardado no navegador. Use HTTPS quando a aplicação sair do localhost.
- Não há bloqueio de tentativas de login (proteção contra força bruta). Recomendo adicionar antes de expor a aplicação na rede.

## QUALITYCORE v0.18.0 — Detalhe por API dos monitores e edição de nome da Collection

### Dashboard: latência por monitor

- Cada monitor da legenda do gráfico de latência é clicável. Ao clicar em um monitor de Collection, abre-se, abaixo do gráfico, o detalhe das APIs daquele monitor: um gráfico com uma linha por API (latência em ms ao longo das verificações) e uma tabela com a última execução de cada uma (resultado, código HTTP e latência).
- Monitores de API test mostram uma mensagem indicando que executam uma única API.

### Monitors

- Cada card de monitor de Collection ganha um painel "APIs da Collection", com a latência de cada API e a última execução.

### Collections: editar o nome

- Na tela de edição de etapas, o campo **Nome da Collection** permite renomear a Collection.
- Se já existir outra Collection com o mesmo nome no projeto (sem diferenciar maiúsculas e minúsculas), o salvamento é bloqueado e o usuário vê a mensagem "Já existe uma Collection com este nome neste projeto". O servidor também recusa o nome duplicado, com a mesma mensagem.

### Dados gravados

- Cada verificação de monitor de Collection guarda o resultado de cada API do fluxo (coluna `stepsJson`, na tabela `monitor_checks`). Verificações anteriores à 0.18.0 não têm esse detalhe, e a tela mostra as APIs somente a partir da primeira verificação desta versão.

### Arquivos alterados

- Back-end: `model/MonitorCheck.java`, `service/MonitorScheduler.java`, `api/LatencyController.java`.
- Front-end: `static/app.js`, `static/latency-chart.js`, `static/styles.css`.
- Testes: `MonitorSchedulerTest` (Java) e `steps.test.js` (interface).

### Validação

- Testes Java: 88 passando, incluindo a gravação das APIs de cada verificação de Collection.
- Testes de interface: clique na legenda, detalhe por API, painel no card de Monitors, edição de nome e bloqueio de nome duplicado.

## QUALITYCORE v0.17.1 — Ajustes de Collections, performance e planejamento de segurança

### Collections

- **Editar etapas:** o dropdown "Add another API test from this project" agora mostra as APIs do projeto que não estão na sequência que está na tela. Antes, ele era calculado uma única vez, ao abrir a edição. Assim, ao remover todas as etapas, nenhuma API aparecia, mesmo existindo APIs no projeto.
- **Alinhamento:** a lista de APIs do formulário de Collection fica alinhada à esquerda.

### Performance

- **Métricas finais** em colunas que se ajustam à largura da tela, no lugar de uma coluna única.
- **Relatório:** o botão só fica habilitado depois que o teste termina (PASS, FAIL ou ERROR). O endpoint `GET /api/performance/runs/{id}/report` também recusa, com 409, um teste que ainda não terminou.

### Segurança: decisão de escopo

- O primeiro alvo dos testes de segurança será a **própria API do QUALITYCORE**, porque é a única cujas rotas conhecemos com precisão.
- **JWT:** não se aplica a ela. O QUALITYCORE não tem autenticação: o `pom.xml` não inclui nenhuma dependência de segurança, e nenhuma rota exige token.
- Achados já visíveis no código, a serem confirmados por testes:
  - Qualquer cliente que alcance a aplicação consegue listar, criar, alterar e excluir APIs, Collections e monitores.
  - Qualquer cliente consegue iniciar testes de carga, e finalizá-los.
  - As APIs de teste aceitam qualquer URL. Como o servidor faz as requisições, isso pode ser usado para acessar endereços internos a partir do servidor (SSRF).
- Recomendação: deixar a aplicação acessível apenas em localhost, ou colocar autenticação antes de expô-la na rede. A autenticação é o próximo passo, se você quiser implementá-la.

### Arquivos alterados

- `static/app.js`, `static/styles.css`, `PerformanceService.java`, `PerformanceReport.java`, `PerformanceValidationTest.java`, `pom.xml`, `LICENSE` e `README.md`.

### Validação

- Testes Java: 88 passando, incluindo o bloqueio do relatório de teste em andamento.
- Testes de interface: todas as suítes passam. Reproduzi o defeito do dropdown antes de corrigir e confirmei a correção depois.

## QUALITYCORE v0.17.0 — Renomeação, métricas de servidor, relatórios e ajustes de tela

### Renomeação

- O produto passa a se chamar **QUALITYCORE** em toda a ferramenta: título, marca, textos, nome da aplicação, pacote Java (`com.qualitycore`) e artefato Maven (`qualitycore`).
- **Banco de dados:** o arquivo continua sendo `./data/qasentinel`, para que os dados já existentes sejam mantidos. Trocar o nome exigiria copiar o arquivo manualmente.
- O tema salvo no navegador voltou ao padrão uma vez, porque a chave local foi renomeada.

### Métricas de `check()` do k6

- A taxa de sucesso de cada `check()` do script aparece por segundo (gráfico) e no total, por verificação (aprovados / total e percentual).
- Validado com um teste real: 6.724 de 6.724 checks aprovados, igual ao número de requisições.

### Infraestrutura do servidor alvo

- Campo opcional **URL do servidor alvo para métricas** no formulário. Com ela, a aplicação lê, uma vez por segundo, pelo Spring Boot Actuator (`/actuator/metrics`):
  - CPU do processo (%);
  - memória usada pela JVM (MB);
  - conexões ativas e pendentes do pool do banco (HikariCP).
- O alvo precisa expor o endpoint de métricas (`management.endpoints.web.exposure.include` com `metrics`). Métricas que o alvo não expõe são ignoradas.
- As métricas aparecem em gráficos próprios e no relatório.
- Validado com um servidor que imita o Actuator, durante uma execução real do k6.

### Finalizar execução

- Botão **Finalizar** para execuções em andamento. O k6 para pela própria API REST, e os resultados parciais são mantidos. O veredito usa os mesmos limites de p95 e de erro.
- Validado: execução finalizada no meio, com o resumo gerado e o veredito calculado.

### Pausa

- **Não disponível nesta versão.** Os testes do k6 v2.3.0 recusam a pausa para os executores usados pelos cenários (`constant-vus` e `ramping-vus`), e o executor que aceita pausa não está disponível neste build.

### Relatório para impressão e PDF

- Botão **Relatório** em cada execução concluída ou em andamento. Ele abre uma página com identificação, veredito, métricas, checks, gráficos de evolução, infraestrutura e a nota de direitos autorais.
- O botão **Imprimir / Salvar como PDF** da própria página usa a função de impressão do navegador. Não há gerador de PDF no servidor.

### Ajustes de tela

- **Métricas finais:** exibidas em uma coluna única, uma métrica por linha.
- **Nome do teste:** campo ocupa três colunas do formulário.
- **Tipo de alvo:** seletor segmentado (API test / Collection), no lugar dos botões de rádio.
- **Collections:** URLs longas quebram a linha dentro da etapa, sem deformar a lista.

### Settings

- Três abas: **Aplicação** (com a verificação de saúde), **Versão** (versão atual, atualizada a cada release) e **Propriedade intelectual** (texto de direitos autorais e restrições de uso).
- O mesmo texto está no arquivo `LICENSE` na raiz do projeto.

### Arquivos alterados

- Backend (novos): `service/TargetMetricsSampler.java`, `service/PerformanceReport.java`.
- Backend (alterados): `service/K6TimelineAggregator.java`, `service/PerformanceService.java`, `api/PerformanceController.java`, `model/PerformanceRun.java`, renomeação de pacote e classes em todo o código.
- Front-end: `static/app.js`, `static/styles.css`, `static/index.html`, `static/chart-tooltip.js`, `static/latency-chart.js`, `static/perf-charts.js`, `static/assertion-builder.js`, `static/execution-checks.js`.
- Raiz: `LICENSE` (novo), `pom.xml`, `README.md`.

### Validação

- Testes Java: 87 passando, incluindo o agregador (checks, bytes, infraestrutura), o amostrador (com um servidor HTTP real), o relatório (escape de valores, seções condicionais) e as regras de parada e de validação.
- Testes de interface: painel de performance, formulário, Settings e as suítes das versões anteriores.
- Execução real com o k6: parada pela API no meio do teste, com infraestrutura lida de um servidor que imita o Actuator. Um erro encontrado nessa validação foi corrigido: a parada manual estava sendo julgada como falha mesmo dentro dos limites.

### Pendente de validação no seu ambiente

- `mvn clean test`, com as dependências reais.
- Renomeação do pacote: conferir que a aplicação sobe e que a tela, os testes e as Collections funcionam como antes.
- Um alvo Spring Boot com Actuator habilitado, para validar as métricas reais de CPU, memória e banco.

## QUALITYCORE v0.16.0 — Acompanhamento ao vivo e métricas de performance

### Painel de acompanhamento

- Ao clicar em **Ver** numa execução, o detalhe mostra KPIs (usuários virtuais, req/s, % de erro e p95) e cinco gráficos com o mesmo eixo de tempo:
  - **Volume:** requisições por segundo.
  - **Usuários virtuais:** quantos estão ativos em cada segundo.
  - **Latência por percentil:** média, p90, p95 e p99.
  - **Status HTTP por segundo:** 2xx, 3xx, 4xx, 5xx e outros, empilhados.
  - **Decomposição do tempo:** bloqueio, conexão TCP, TLS, envio, espera (TTFB) e recebimento, em média por segundo. Mostra se a lentidão está na API ou na rede.
- Enquanto a execução está em andamento, o painel se atualiza a cada 3 segundos, com o aviso "atualizando ao vivo".
- Ao terminar, aparece a tabela de **métricas finais** (média, mínimo, máximo, p50, p90, p95, p99, req/s e taxa de erro), com cada limite marcado como atendido ou não.
- Passar o cursor sobre qualquer gráfico mostra os valores de todas as séries naquele segundo.

### Como as métricas são coletadas

- O k6 grava cada medição (ponto) num **pipe** (canal interno do sistema). A aplicação lê esse canal e agrega os pontos por segundo, sem gravar os pontos brutos. Um teste longo não ocupa espaço em disco.
- Onde não há pipe (Windows), o k6 grava num arquivo temporário, que é lido ao final e apagado.
- A linha do tempo final fica salva em `data/perf/run-<id>/timeline.json`.
- Validado: os totais, a média, o p90, o p95 e o máximo calculados pela aplicação batem com o resumo oficial do k6 no mesmo teste.

### Formulário

- Os campos ficam em grade horizontal, em vez de uma coluna longa.

### Endpoint novo

- `GET /api/performance/runs/{id}/timeline`: linha do tempo da execução. Durante a execução, devolve o que já foi medido; depois, o arquivo salvo.

### Arquivos alterados

- Backend (novos): `service/K6TimelineAggregator.java`.
- Backend (alterados): `service/PerformanceService.java`, `api/PerformanceController.java`.
- Front-end (novos): `static/perf-charts.js`.
- Front-end (alterados): `static/app.js`, `static/index.html`, `static/styles.css`.
- Configuração: `pom.xml` e `README.md`.

### Validação

- Testes Java: 75 passando, incluindo o agregador (agrupamento por segundo, percentis, códigos de status, falhas, fases, linhas malformadas e crescimento durante a execução).
- Execução real com o k6: a linha do tempo cresceu durante os 20 segundos do teste (de 0 a 20 buckets) e o usuário virtual subiu de 1 para 5 e desceu no fim. O resultado final foi PASS, com o pipe removido e o `timeline.json` salvo.
- Testes de interface: formulário, painel ao vivo, execução em andamento e métricas finais.

### Pendente de validação no seu ambiente

- `mvn clean test` com as dependências reais.
- Um teste longo (por exemplo, 5 minutos) para conferir o consumo de memória e a fluidez dos gráficos.
- No Windows, a execução usa o arquivo temporário e o acompanhamento ao vivo não é feito durante o teste.

## QUALITYCORE v0.15.0 — Testes de performance com k6, tooltips e versão

### Testes de performance (novo menu "Performance")

- **Alvos:** uma API test ou uma Collection inteira. Uma Collection roda as etapas na ordem salva, dentro de cada usuário virtual.
- **Cenários:**
  - **SMOKE:** 1 usuário virtual durante toda a execução, para validar o fluxo antes de carregar.
  - **LOAD (carga):** sobe até o máximo, mantém e desce.
  - **STRESS:** aumenta em degraus (25%, 50%, 75% e 100% do máximo) até achar o limite.
  - **SPIKE (pico):** salta para o máximo em poucos segundos e volta.
- **Limites de segurança:** até 50 usuários virtuais e 600 segundos por execução, configuráveis; no mínimo 10 segundos. Uma execução é feita por vez, com até 5 na fila.
- **Confirmação:** cada execução exige a caixa "Entendo que esta execução envia carga real" e um diálogo que mostra alvo, cenário, usuários e duração. Sem os dois, nada é enviado.
- **Veredito:** PASS quando o k6 cumpre os limites de p95 e de erro; FAIL quando algum limite é ultrapassado (a mensagem diz qual e o valor medido); ERROR quando o k6 não consegue executar. Uma execução sem nenhuma requisição concluída nunca é PASS.
- **Métricas:** requisições, requisições por segundo, média, p90, p95, máximo, taxa de erro e usuários virtuais no pico.

### k6: instalação automática

- Se houver `qa.k6.path` configurado, usa esse binário.
- Senão, procura `k6` no PATH.
- Senão, usa a cópia já baixada em `data/perf/tools`.
- Senão, baixa a versão mais recente das releases oficiais do GitHub e confere o sha256 com o arquivo de checksums publicado na mesma release antes de usar.
- Para atualizar para uma versão mais nova, apague a pasta `data/perf/tools` e rode uma execução.

Validado: o download da versão v2.3.0 (checksum conferido) e execuções reais com o k6 contra um servidor local, com PASS e FAIL.

**Ainda não validado:** o download automático foi testado só em Linux amd64. Em Linux arm64 o padrão do arquivo é o mesmo, mas não foi testado. Em macOS e Windows, instale o k6 manualmente e configure `qa.k6.path`.

### Configuração (application.properties)

- `qa.k6.path`: caminho do binário do k6 (vazio = automático).
- `qa.perf.max-vus`: limite de usuários virtuais (padrão 50).
- `qa.perf.max-duration-seconds`: limite de duração (padrão 600).
- `qa.perf.data-dir`: pasta de scripts, resumos, logs e do k6 baixado (padrão `./data/perf`).

### Dashboard

- **Tooltips nos gráficos:** ao passar o mouse, aparece o valor do ponto ou da fatia: uptime das APIs, tempo de resposta, status das APIs e distribuição de códigos HTTP. No gráfico de latência por monitor, o tooltip mostra, para o instante do cursor, o valor mais próximo de cada monitor, com o horário.
- **Últimas execuções:** a coluna "Status" foi removida, porque mostrava a mesma informação de "Resultado". Agora há uma coluna só, com o mesmo badge PASS/FAIL das outras tabelas.

### Settings

- Mostra a versão que está em validação: **0.15.0**.

### Arquivos alterados

- Backend (novos): `model/PerformanceRun.java`, `repo/PerformanceRunRepository.java`, `service/K6ScriptBuilder.java`, `service/K6Summary.java`, `service/K6Installer.java`, `service/PerformanceService.java`, `api/PerformanceController.java`.
- Front-end: `static/app.js`, `static/index.html`, `static/styles.css`, `static/chart-tooltip.js` (novo), `static/latency-chart.js`.
- Configuração: `application.properties`, `pom.xml` e `README.md`.

### Validação

- Testes Java: 64 passando, incluindo o roteiro de cada cenário, a escaping de valores vindos da configuração, a leitura do resumo do k6, o checksum, os limites de segurança e as mensagens de limite ultrapassado.
- Execução real com o k6 v2.3.0: PASS num cenário saudável e FAIL com HTTP 500, com o motivo correto. Os testes de execução de ponta a ponta encontraram dois defeitos, já corrigidos: o Smoke não começava com usuário ativo, e uma execução sem requisições podia virar PASS.
- Testes de interface: performance (formulário, confirmação, lista e detalhe), tooltips do dashboard, tabela de resultados, Settings e regressão das versões anteriores.

### Pendente de validação no seu ambiente

- `mvn clean test` com as dependências reais.
- Uma execução de cada cenário na aplicação em execução, conferindo os gráficos e o detalhe.
- Primeira execução numa máquina sem k6, para confirmar o download automático.

## QUALITYCORE v0.14.0 — Uma API em várias Collections e alvo único no monitor

- **Uma API pode estar em várias Collections.** A pertença passa a ser definida pela lista ordenada de cada Collection. Antes, cada API guardava uma única Collection, e cadastrar a mesma API em outra Collection retornava 409. Esse 409 não existe mais.
- **A execução de uma Collection usa a sua própria sequência.** A mesma API roda em todas as Collections que a incluem, sem que uma execução tire a API da outra.
- **Excluir uma API** a remove automaticamente das Collections que a usavam, para não deixar etapas órfãs.
- **Formulário de API Test:** o campo de texto "Collection (optional)" foi substituído pela lista somente leitura das Collections em que a API está.
- **Builder de Collection:** as etapas aparecem na ordem salva da própria Collection.
- **Formulário de monitor:** um único campo de alvo. Ao escolher "API test" ele lista as APIs; ao escolher "Collection", lista as Collections. O rótulo acompanha o tipo.

### Compatibilidade

- Os dados existentes continuam funcionando. O campo `collectionName` das APIs fica no banco, mas não é mais a fonte da pertença.
- Coleções já salvas mantêm a sequência e a política de falha.
- A execução por nome (`POST /api/collections/run` sem `orderedTestIds`) usa a sequência salva da Collection com aquele nome no projeto.

### Arquivos alterados

- Backend: `api/CollectionController.java`, `api/TestController.java`, `service/CollectionExecutionService.java`.
- Front-end: `static/app.js`.
- Testes novos: `CollectionMembershipTest` (Java) e `shared.test.js` (interface).

### Validação

- Testes Java: 42 passando, incluindo 4 novos sobre pertencimento compartilhado, criação sem 409 e execução por nome.
- Testes de interface: monitores (campo único), pertencimento compartilhado na tabela, no builder, nas quick actions e no formulário de API Test, além da regressão da v0.11.1.

### Pendente de validação no seu ambiente

- `mvn clean test` com as dependências reais.
- Criar uma API, incluí-la em duas Collections e executar as duas, conferindo a sequência de cada uma.
- Excluir uma API usada em uma Collection e conferir se ela sai da lista.

## QUALITYCORE v0.13.1 — Ajuste visual do gráfico de latência

- O gráfico de latência agora é desenhado na largura real do painel (1 unidade = 1 px). Antes, era escalado a partir de uma área fixa de 900 px, o que deixava textos e linhas grandes em telas menores.
- Linhas mais finas (1.25 px) e rótulos de eixo em 10 px, no mesmo padrão dos demais gráficos do dashboard.
- O gráfico é redesenhado ao redimensionar a janela.

Arquivos alterados: `static/latency-chart.js`, `static/styles.css`, `pom.xml` e `README.md`. Nenhuma mudança no backend nem no banco.

## QUALITYCORE v0.13.0 — Monitores com Collections e gráfico de latência

- **Monitor com Collection:** ao criar um monitor, escolha o alvo: uma API test ou uma Collection já cadastrada. Um monitor tem exatamente um alvo. Quando o alvo é uma Collection, o scheduler executa todas as etapas na ordem salva e com a política de falha da Collection.
- **Resultado do monitor de Collection:** UP quando todas as etapas executadas passaram; DOWN caso contrário. A latência registrada é a soma das etapas executadas, ou seja, o tempo de ponta a ponta do fluxo. Etapas puladas não contam.
- **Histórico de verificações:** cada verificação de monitor (de teste ou de Collection) é gravada na tabela `monitor_checks`, com status, latência e código HTTP quando houver.
- **Dashboard, painel "Latência por monitor":** gráfico de linha por monitor com as verificações da janela escolhida (1 h, 24 h ou 7 dias). A legenda mostra média, p95, máximo, disponibilidade e número de verificações de cada monitor. Verificações DOWN aparecem marcadas em vermelho, e cada ponto tem tooltip com monitor, horário, latência e status.
- **Monitores:** o gráfico de monitores de Collection mostra a latência do fluxo nas últimas 24 h. Monitores de API test mantêm o gráfico de execuções existente.

### Mudança no banco de dados

- Tabela `monitors`: nova coluna `collectionId`. Monitores existentes continuam apontando para o `testId` deles.
- Nova tabela `monitor_checks`, com índice por monitor e horário.

Como o `spring.jpa.hibernate.ddl-auto` está em `update`, ambas são criadas automaticamente. O histórico de latência começa a ser gravado a partir desta versão; execuções anteriores não geram pontos no gráfico.

### API

- `POST /api/monitors`: aceita `testId` **ou** `collectionId`. Enviar os dois, ou nenhum, retorna 400.
- `GET /api/dashboard/latency?hours=24`: série de latência por monitor, com estatísticas. Janela de 1 a 168 horas; no máximo 2000 pontos por monitor, sempre os mais recentes.

### Limitações conhecidas

- As etapas de uma Collection disparada por monitor usam o timeout fixo de 10 s, não o timeout do monitor.
- O histórico de `monitor_checks` não tem limpeza automática. Em uso prolongado, convém criar uma rotina de retenção.
- O gráfico mostra as verificações gravadas a partir desta versão.

### Arquivos alterados

- Novos: `model/MonitorCheck.java`, `repo/MonitorCheckRepository.java`, `service/LatencyStats.java`, `api/LatencyController.java`, `static/latency-chart.js`, e os testes `LatencyStatsTest`, `MonitorControllerTest` e `MonitorSchedulerTest`.
- Alterados: `model/Monitor.java`, `api/MonitorController.java`, `service/MonitorScheduler.java`, `static/app.js`, `static/index.html`, `static/styles.css`, `pom.xml` e `README.md`.

### Validação

- Testes Java: 38 testes passando, incluindo 19 novos para estatísticas, validação de alvo do monitor e scheduler nos dois tipos de alvo (sucesso, falha, erro, teste pausado e intervalo não vencido).
- Testes de interface (jsdom): criação de monitor por Collection, página de Monitors para os dois tipos de alvo, painel de latência no Dashboard, janelas de tempo, escape de HTML e regressão da v0.11.1.

### Pendente de validação no seu ambiente

- `mvn clean test` com as dependências reais.
- Execução real: criar um monitor de Collection, aguardar algumas verificações e conferir o gráfico no Dashboard nos temas claro e escuro.
- Reinício com um banco da v0.12.0, para confirmar a criação de `collectionId` e `monitor_checks`.

## QUALITYCORE v0.12.0 — Assertion Engine, relatórios e tempo de resposta

Esta entrega melhora a execução e o detalhamento dos resultados. Todas as regras de decisão agora ficam em um único lugar (`ResponseChecker`), usado tanto pela execução individual quanto pela Collection.

- **Assertion Engine:** cada assertion produz um resultado estruturado com tipo, descrição, operador, valor esperado, valor obtido, resultado e motivo. Um erro em uma assertion (por exemplo, JSONPath inválido) reprova só aquela assertion; as demais continuam sendo avaliadas.
- **Collection Reports:** o relatório mostra, para cada etapa, o status HTTP, o tempo de resposta e cada assertion com esperado e obtido. Os motivos das falhas aparecem em lista.
- **Expected vs Actual:** status HTTP, tempo de resposta e assertions mostram o valor esperado e o valor obtido lado a lado. Valores ausentes aparecem como `(ausente)`. Valores de resposta são exibidos com escape, sem injeção de HTML.
- **Response Time:** o limite de tempo (`maxResponseTimeMs`) é validado em execuções individuais e em Collections com a mesma regra. O limite é inclusivo: resposta exatamente no limite passa. Mesmo sem limite configurado, o tempo medido é registrado.
- **Consistência:** se uma etapa de Collection falhar depois das checagens (por exemplo, na extração de variável), ela aparece como FAIL também nas verificações. Antes, o relatório podia mostrar tudo verde com a etapa vermelha.
- **Regressão:** os recursos da v0.11.1 (Query Parameters, Headers, Request Preview, métodos HEAD e OPTIONS, autenticação, body, timeout e Collections) mantêm o mesmo comportamento. Só o bloco de checagem foi alterado nos serviços de execução.

### Mudança no banco de dados

A tabela `executions` ganhou a coluna `checksJson`, que guarda o resultado estruturado de cada execução. Como o `spring.jpa.hibernate.ddl-auto` está em `update`, a coluna é criada automaticamente na primeira inicialização. Execuções anteriores continuam listadas com o detalhe textual; a seção de verificações mostra que os dados detalhados não estão disponíveis para elas.

Se o JSON de checagens for maior que o limite da coluna, o sistema salva uma versão reduzida, com o veredito, o status, o tempo e as assertions que falharam.

### Arquivos alterados

- Novos: `service/AssertionResult.java`, `service/ExecutionChecks.java`, `service/ResponseChecker.java`, `static/execution-checks.js` e `src/test/.../ResponseCheckerTest.java`.
- Alterados: `service/AssertionEvaluator.java`, `service/ExecutionService.java`, `service/CollectionExecutionService.java`, `model/Execution.java`, `static/app.js`, `static/index.html`, `static/styles.css`, `pom.xml` e `README.md`.

### Validação

- Testes unitários do `ResponseChecker` (20 testes no total, incluindo os de armazenamento): status, limite de tempo com fronteira exata, JSONPath, header, texto no corpo, schema, configuração inválida, isolamento de erro por assertion e formato reduzido.
- Teste de mutação: alterar a fronteira do limite de tempo faz o teste correspondente falhar.
- Testes de interface (jsdom): criação e edição de assertions, validação, renderização de verificações na aba Executions, no modal e no relatório da Collection, escape de HTML e regressão dos recursos da v0.11.1.
- Compilação de todo o código Java, usando stubs apenas para Spring, JPA e REST Assured.

### Pendente de validação no seu ambiente

- `mvn clean test` com as dependências reais. Os testes unitários novos usam JUnit 5 e Mockito, já incluídos no `spring-boot-starter-test`.
- Execução real contra uma API, criando e executando API Tests e Collections com limite de tempo, assertions e extração de variáveis, e conferindo o relatório no navegador nos temas claro e escuro.
- Reinício da aplicação com um banco existente da v0.11.x, para confirmar a criação automática da coluna `checksJson`.

## QUALITYCORE v0.11.2 — Assertions pela interface

Esta entrega cria o construtor visual de Assertions. O campo de JSON livre foi substituído por linhas editáveis; nenhum JSON precisa ser digitado à mão.

- Adicionar, editar e remover Assertions no formulário de API Test, sem escrever JSON.
- Tipos suportados: `status` (código HTTP), `header` (nome e valor), `jsonPath` (caminho e operador `é igual a`, `contém` ou `existe`), `contains` (texto no corpo) e `schema` (tipo da raiz, propriedades obrigatórias e tipo de cada propriedade).
- Validação antes de salvar: campos obrigatórios, código HTTP entre 100 e 599 e nomes de propriedades não repetidos. Erros são mostrados na tela e impedem o envio.
- Prévia do JSON que será salvo, atualizada a cada alteração.
- Registros já salvos são carregados nas linhas. Operador `exists` antigo é exibido como `existe`. Assertions de tipo que a interface não edita são preservadas sem alteração e exibidas como somente leitura.
- O formato persistido em `assertionsJson` e o `AssertionEvaluator` não mudaram. Testes e Collections existentes continuam funcionando.

Arquivos alterados: `static/assertion-builder.js` (novo), `static/app.js`, `static/index.html`, `static/styles.css`, `pom.xml` e `README.md`. O backend Java não foi alterado.

### Validação

- `node --check` passa em `app.js` e `assertion-builder.js`.
- Testes de interface com jsdom cobrem: criação das cinco assertions, validação e bloqueio de envio, envio do JSON, reabertura, edição, remoção e registros legados (operador `exists`, tipo desconhecido e JSON inválido).
- `mvn clean test` e a validação manual no navegador (criar, editar, salvar, reabrir e executar API Tests e Collections) ainda precisam ser feitas no ambiente de desenvolvimento. A entrega não deve ser considerada aprovada até essa validação.

## QUALITYCORE v0.11.1 — Query Parameters, Headers e Request Preview

Esta entrega parte da v0.10 e preserva o layout existente.

- Query Parameters em linhas editáveis, com inclusão/remoção e codificação por `URLSearchParams` na prévia.
- Headers em linhas editáveis, com inclusão/remoção. Valores de headers sensíveis são exibidos como campo de senha; a prévia não exibe valores de headers.
- Request Preview acompanha método, URL e parâmetros.
- Os dados continuam persistidos nos campos JSON existentes (`queryParamsJson` e `headersJson`), preservando compatibilidade com testes antigos. Ao editar registros antigos, os objetos JSON existentes são carregados nas linhas editáveis.
- Métodos HTTP `HEAD` e `OPTIONS` disponíveis na execução individual e em Collections.
- Autenticação `NONE`, `BEARER` e `BASIC`, body, timeout e Assertions existentes foram mantidos.

### Validação

O arquivo JavaScript deve passar em `node --check`; a suíte Maven precisa ser executada no ambiente de desenvolvimento (`mvn clean test`). A entrega não deve ser considerada aprovada até validar no navegador a criação, edição, salvamento, reabertura e execução de API Tests e Collections.
