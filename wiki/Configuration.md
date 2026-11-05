# Configuração

As configurações padrão estão em `src/main/resources/application.properties`.

| Propriedade | Valor padrão | Descrição |
|---|---|---|
| `server.port` | `8080` | Porta HTTP |
| `spring.datasource.url` | `jdbc:h2:file:./data/qasentinel;AUTO_SERVER=TRUE` | URL do banco H2 |
| `spring.datasource.username` | `sa` | Usuário do H2 local |
| `spring.datasource.password` | vazio | Senha do H2 local |
| `spring.jpa.hibernate.ddl-auto` | `update` | Atualização do schema |
| `spring.h2.console.enabled` | `true` | Habilita console H2 |
| `spring.h2.console.path` | `/h2-console` | Caminho do console |
| `qa.k6.path` | vazio | Caminho explícito do binário k6; vazio procura no PATH e, se não encontrar, tenta obter uma versão |
| `qa.perf.max-vus` | `50` | Máximo de usuários virtuais |
| `qa.perf.max-duration-seconds` | `600` | Duração máxima em segundos |
| `qa.perf.data-dir` | `./data/perf` | Diretório de dados de performance |
| `qa.auth.secret` | vazio | Segredo JWT HS256 |
| `qa.auth.token-hours` | `8` | Validade do token |

## Recomendações

- Defina `qa.auth.secret` como segredo fixo, aleatório e com pelo menos 32 caracteres.
- Não armazene segredos no Git.
- Não exponha o console H2 sem revisar autenticação, rede e firewall.
- Ajuste os limites de performance de acordo com os recursos da máquina e a autorização sobre o alvo.
- Mantenha cópias de segurança do diretório `data/`.

## Observação sobre autenticação

Com `qa.auth.secret` vazio, a chave é aleatória por inicialização; reiniciar o servidor invalida os tokens emitidos anteriormente.
