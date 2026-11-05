# Settings

A área Settings reúne preferências de interface e gerenciamento da conta.

## Tema

A aplicação oferece alternância entre tema claro e escuro.

## Conta

Na aba Conta:
- É possível encerrar a sessão.
- É possível criar novos usuários.

## Modelo de autorização

Na versão 0.19.2, todos os usuários têm os mesmos poderes. Não existem perfis, papéis ou permissões granulares.

## Autenticação

A aplicação usa JWT HS256. A validade padrão é de 8 horas e pode ser configurada por `qa.auth.token-hours`.

Consulte [Security](Security) para detalhes e recomendações.
