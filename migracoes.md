```markdown
# Migrações em banco de dados PostgreSQL

As migrações de banco de dados permitem modificar o esquema de uma base de dados de forma controlada e reproduzível. Elas são fundamentais para manter a consistência e evitar problemas com a evolução de aplicações que dependem de um banco de dados PostgreSQL.

## Introdução às migrações

Em uma aplicação em produção, é comum que o esquema do banco de dados sofra mudanças. Essas alterações podem incluir:

- Adição ou remoção de tabelas e colunas;
- Alterações em tipos de dados;
- Criação de índices ou novas constraints;
- Ajustes de permissões de usuários, entre outros.

Utilizar migrações permite controlar essas mudanças de forma segura, gerando um histórico versionado do banco de dados e facilitando o rollback em caso de problemas.

## Estrutura de uma migração

Uma migração é geralmente composta por:

1. **Um script de "up"**: responsável por aplicar a mudança no banco.
2. **Um script de "down"**: usado para reverter a alteração (rollback).

Exemplo de estrutura básica de uma migração em SQL:

```sql
-- Migração: Adicionar coluna "email" na tabela "users"

-- UP
ALTER TABLE users ADD COLUMN email VARCHAR(255);

-- DOWN
ALTER TABLE users DROP COLUMN email;
```

## Ferramentas para gerenciamento de migrações

Em PostgreSQL, o gerenciamento de migrações pode ser feito com ferramentas específicas, como:

- **Flyway**: simples de configurar e usar, permite definir migrações em SQL puro ou Java.
- **Liquibase**: oferece suporte a XML, YAML, JSON e SQL para definir migrações, além de recursos avançados como "changelogs".
- **Node.js (Knex)**, **Django** ou **Rails**: esses frameworks possuem suas próprias ferramentas de migração integradas, como `Knex migrations`, `Django migrations` e `ActiveRecord migrations`, respectivamente.

### Exemplo com Flyway

1. Instale o Flyway:

   ```bash
   brew install flyway
   ```

2. Configure o arquivo `flyway.conf`:

   ```properties
   flyway.url=jdbc:postgresql://localhost:5432/meubanco
   flyway.user=meuusuario
   flyway.password=minhasenha
   ```

3. Crie uma migração SQL em `sql/V1__create_users_table.sql`:

   ```sql
   CREATE TABLE users (
       id SERIAL PRIMARY KEY,
       name VARCHAR(100) NOT NULL
   );
   ```

4. Execute a migração:

   ```bash
   flyway migrate
   ```

## Criação de migrações em SQL

Para realizar mudanças no esquema de forma manual, basta escrever scripts SQL e executá-los diretamente no banco. No PostgreSQL, a estrutura de uma migração típica inclui comandos como `ALTER TABLE`, `CREATE INDEX`, `DROP TABLE`, entre outros.

### Exemplo de migração para adicionar uma tabela

```sql
-- Migração: Criação da tabela "orders"
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2) NOT NULL
);
```

### Exemplo de migração para alterar uma coluna

```sql
-- Migração: Modificação da coluna "name" na tabela "users"
ALTER TABLE users ALTER COLUMN name TYPE VARCHAR(150);
```

## Controle de transações em migrações

Para garantir consistência, é essencial que as migrações sejam executadas em uma transação. Se algum passo falhar, o PostgreSQL permite o **rollback** de toda a transação, evitando que o banco fique em um estado intermediário.

```sql
BEGIN;

-- Exemplo de migração com transação
ALTER TABLE users ADD COLUMN age INTEGER;

COMMIT;
```

## Rollbacks e reversão de migrações

Para reverter uma migração, é necessário definir o script de rollback correspondente. Esse script deve desfazer as alterações feitas pela migração, permitindo restaurar o estado anterior do banco.

Exemplo de um rollback para remover uma tabela:

```sql
-- Rollback: Remoção da tabela "orders"
DROP TABLE IF EXISTS orders;
```

## Boas práticas em migrações de banco de dados

- **Testar as migrações em ambiente de desenvolvimento** antes de aplicá-las em produção;
- **Controlar a ordem de execução** das migrações, assegurando que elas sigam uma sequência lógica;
- **Versionar scripts de migração** no sistema de controle de versão do projeto;
- **Evitar mudanças destrutivas** (como `DROP COLUMN`) sem um processo de validação e backups;
- **Escrever scripts de rollback** para permitir reverter as mudanças caso necessário;
- **Usar transações** para manter a consistência e garantir que mudanças parciais não sejam aplicadas.

## Conclusão

As migrações são fundamentais para gerenciar a evolução do banco de dados de forma segura e escalável. Com o uso adequado de ferramentas, transações e boas práticas, é possível manter o banco de dados PostgreSQL alinhado com as mudanças da aplicação e evitar problemas de integridade.

```
