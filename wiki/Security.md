## Controles implementados na versão 0.19.2

- Autenticação por JWT HS256.
- Senhas armazenadas com PBKDF2-HMAC-SHA256, 210 mil iterações e salt aleatório por senha.
- Validação de assinatura, expiração e algoritmo do JWT.
- Rejeição de tokens adulterados e de `alg: none`.
- Setup inicial para criação da primeira conta.
- Mensagem genérica de falha de login para usuário inexistente ou senha incorreta.
- Proteção das rotas `/api/**`, com exceções necessárias para health, status/setup e login.
- Sessão expirada tratada pelo frontend, retornando à tela de login.

## Configuração obrigatória

Defina `qa.auth.secret` com valor fixo, aleatório e de pelo menos 32 caracteres. Não versionar esse segredo.

Use HTTPS quando a aplicação deixar de ser exclusivamente local.

## Limitações de segurança conhecidas

- Não há bloqueio/rate limiting de tentativas de login.
- Não há perfis ou autorização granular: todos os usuários possuem os mesmos poderes.
- O token é armazenado no navegador.
- O MVP não deve ser exposto publicamente sem hardening.
- SSRF, proteção de destinos, gestão de segredos, limites de requisição e controles operacionais precisam de avaliação antes de uso compartilhado.
- Console H2 está habilitado por padrão na configuração fornecida.

## Uso responsável

A aplicação pode realizar requisições a destinos definidos pelo usuário. Restrinja testes e monitores a alvos autorizados e proteja credenciais e dados retornados.
