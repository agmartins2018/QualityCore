# Instalação e execução

## Pré-requisitos

- JDK 21.
- Maven 3.9 ou compatível.
- Navegador atualizado.
- Conectividade com os alvos de API que serão testados.
- k6 para testes de performance (o projeto contém um instalador/localizador de k6).

## Executar em desenvolvimento

No diretório do `pom.xml`:

```bash
mvn clean test
mvn spring-boot:run
```

A aplicação inicia por padrão na porta `8080`.

Abra:

```text
http://localhost:8080
```

## Gerar pacote

```bash
mvn clean package
```

O Spring Boot Maven Plugin gera o artefato executável no diretório `target/`.

## Diretórios de dados

- Banco H2: `./data/qasentinel`.
- Dados e ferramentas de performance: `./data/perf`.

O diretório `data/` é ignorado pelo Git, exceto pelo arquivo `.gitkeep`. Faça backup antes de atualizar ou limpar os dados locais.

## Atualização

1. Faça backup de `data/`.
2. Atualize o código para a versão desejada.
3. Execute `mvn clean test`.
4. Inicie a aplicação e valide o fluxo de login.
5. Execute testes representativos de API, Collection, Monitor, Contract e Performance.

O schema JPA está configurado com `ddl-auto=update`. Para ambientes persistentes críticos, adote migrações versionadas e um procedimento testado de backup/restore.

## Problemas comuns

Consulte [Troubleshooting](Troubleshooting).
