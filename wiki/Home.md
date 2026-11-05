# QUALITYCORE Wiki

Bem-vindo à documentação do **QUALITYCORE — API Quality & Performance Engineering Platform**.

<details>
<summary>Sobre esta documentação</summary>

Esta Wiki descreve a versão **0.19.2**, com base no código-fonte e no README incluídos no pacote recebido. Recursos identificados como limitações, recomendações ou evolução futura não devem ser interpretados como funcionalidades já implementadas.

</details>

## Visão geral

O QUALITYCORE reúne, em uma aplicação web local, recursos para testar APIs, organizar execuções, monitorar endpoints e Collections, verificar contratos e executar testes de performance com k6.

## Navegação

- [Visão do produto](Overview)
- [Começando](Getting-Started)
- [Instalação e execução](Installation-Guide)
- [Configuração](Configuration)
- **Documentação funcional**
  - [Dashboard](Dashboard)
  - [Projects](Projects)
  - [API Tests](API-Tests)
  - [Assertions e validação](Assertions)
  - [Collections](Collections)
  - [Executions](Executions)
  - [Monitors](Monitors)
  - [Contracts](Contracts)
  - [Performance Engineering](Performance-Engineering)
  - [Métricas de performance](Performance-Metrics)
  - [Relatórios](Reports)
  - [Settings e contas](Settings)
- **Documentação técnica**
  - [Arquitetura](Architecture)
  - [Tecnologias](Technology-Stack)
  - [Estrutura do código](Codebase-Structure)
  - [Referência REST API](API-Reference)
  - [Modelo de dados](Data-Model)
  - [Estratégia de testes](Testing-Strategy)
- **Operação e governança**
  - [Segurança](Security)
  - [Limitações conhecidas](Known-Limitations)
  - [Troubleshooting](Troubleshooting)
  - [Roadmap](Roadmap)
  - [Release Notes](Release-Notes)
  - [Propriedade intelectual](Intellectual-Property)

## Estado da versão

| Item | Informação |
|---|---|
| Versão | 0.19.2 |
| Java | 21 |
| Spring Boot | 3.5.6 |
| Persistência | H2 + Spring Data JPA |
| Performance | k6 |
| Tipo | MVP local |

## Fluxo de uso sugerido

1. Criar a conta inicial e entrar na aplicação.
2. Criar um Project.
3. Cadastrar API Tests e configurar assertions.
4. Agrupar os testes em Collections quando houver um fluxo sequencial.
5. Executar testes e consultar o histórico.
6. Criar Monitors para verificações recorrentes.
7. Cadastrar Contracts para verificar conformidade de APIs.
8. Executar cenários de Performance e analisar os resultados.

## Aviso

O QUALITYCORE 0.19.2 é um MVP destinado a desenvolvimento e uso local. Consulte [Segurança](Security) e [Limitações conhecidas](Known-Limitations) antes de qualquer exposição em rede.
