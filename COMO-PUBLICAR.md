# Como adicionar a documentação ao GitHub

## 1. README do repositório
Substitua o `README.md` da raiz do repositório pelo `README.md` desta entrega.

## 2. GitHub Wiki
1. No GitHub, abra o repositório e habilite a seção **Wiki** em Settings, se necessário.
2. Abra a Wiki e escolha clonar/editar a Wiki localmente, ou clone o repositório Git da Wiki.
3. Copie **todo o conteúdo** da pasta `wiki/` para a raiz do repositório da Wiki.
4. Faça commit e push.

A pasta `wiki/` contém `Home.md`, `_Sidebar.md` e as páginas Markdown referenciadas pela navegação.

## 3. Observações
- O GitHub Wiki não cria diretórios/subpáginas reais; a hierarquia é representada por páginas e links na `_Sidebar.md`.
- A documentação foi preparada para a versão 0.19.2 e deve ser revisada a cada release.
- O conteúdo diferencia recursos implementados, limitações e evoluções possíveis.
- Não publique segredos, banco local ou dados de execução junto com a documentação.
