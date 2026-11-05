## A aplicação não inicia

- Confirme JDK 21: `java -version`.
- Confirme Maven: `mvn -version`.
- Execute `mvn clean test` e analise a primeira falha.
- Verifique se a porta 8080 já está em uso.
- Confirme se o diretório `data/` permite escrita.

## Login não funciona após reiniciar

Se `qa.auth.secret` estiver vazio, uma chave aleatória é gerada a cada inicialização. Configure um segredo fixo para preservar a validade dos tokens durante reinicializações.

## API retorna 401

- Confirme que o login foi realizado.
- Verifique se o token ainda está válido.
- Faça login novamente se a sessão expirou.
- Confirme se a rota exige autenticação.

## Relatório de performance informa sessão expirada

O relatório deve ser solicitado com a sessão autenticada pelo frontend. Abrir diretamente a rota em uma nova aba sem enviar o token pode resultar em falha de autenticação. Na versão 0.19.2, o fluxo da interface foi corrigido para buscar o relatório autenticado e abri-lo como conteúdo local.

## k6 não é encontrado

- Verifique se o k6 está no PATH.
- Configure `qa.k6.path` com o caminho do executável.
- Confirme conectividade se a instalação automática precisar baixar o binário.
- Verifique permissões de execução e logs em `data/perf`.

## Dados locais parecem ter desaparecido

O banco fica em `./data/qasentinel`, relativo ao diretório de execução. Confirme que está iniciando a aplicação a partir do diretório esperado e que o arquivo de banco não foi removido.

## Monitor de Collection não mostra detalhe antigo por API

O detalhe granular por etapa começou a ser armazenado na versão 0.18.0. Verificações anteriores podem não possuir esse histórico.
