# Getting Started

Este guia apresenta um fluxo inicial para explorar a aplicação.

## 1. Inicie a aplicação

Siga [Instalação e execução](Installation-Guide). Acesse `http://localhost:8080`.

## 2. Configure a primeira conta

Na primeira execução, a aplicação disponibiliza o setup inicial para criar a conta administradora. Depois do setup, entre com as credenciais cadastradas.

## 3. Crie um Project

Acesse **Projects** e crie um projeto para agrupar os testes e Collections relacionados.

## 4. Cadastre um API Test

Em **API Tests**, configure:
- Project associado.
- Método HTTP e URL.
- Query Parameters e Headers, quando necessários.
- Tipo de autenticação suportado.
- Body, se aplicável.
- Timeout e limite de tempo de resposta.
- Assertions para validar a resposta.

## 5. Execute e analise

Execute o teste e consulte o resultado em **Executions**. O resultado inclui status HTTP, tempo de resposta e verificações disponíveis.

## 6. Crie uma Collection

Em **Collections**, selecione API Tests do mesmo Project, ordene as etapas e escolha a política de falha:
- `STOP`: interrompe o fluxo após falha.
- `CONTINUE`: continua para as próximas etapas.

## 7. Explore Monitors, Contracts e Performance

- **Monitors:** acompanhe um API Test ou uma Collection em verificações recorrentes.
- **Contracts:** cadastre um contrato JSON ou importe OpenAPI 3 em JSON e execute a verificação.
- **Performance:** configure e execute um cenário k6 respeitando os limites de segurança configurados.

## Próximas leituras

- [API Tests](API-Tests)
- [Collections](Collections)
- [Monitors](Monitors)
- [Contracts](Contracts)
- [Performance Engineering](Performance-Engineering)
