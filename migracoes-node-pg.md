```markdown
# Migrações em banco de dados PostgreSQL com Node.js e `pg`

Este guia cobre como realizar migrações em um projeto Node.js usando PostgreSQL diretamente, sem bibliotecas intermediárias. 

## Pré-requisitos

- **Node.js** e **npm** instalados.
- **PostgreSQL** em execução e acessível.
- Familiaridade com SQL e PostgreSQL.

## Passo a Passo para Gerenciar Migrações com PostgreSQL Puro

### 1. Instalar as Dependências

Primeiro, instale o cliente `pg` para conectar-se ao PostgreSQL:

```bash
npm install pg
```

### 2. Estrutura de Diretórios

Crie uma estrutura de diretórios para organizar os scripts de migração:

```plaintext
project-root/
├── migrations/
│   ├── 001_create_users_table.sql
│   ├── 002_add_email_to_users.sql
│   └── ...
├── migration-runner.js
└── package.json
```

No diretório `migrations`, cada arquivo SQL representa uma migração. Numere as migrações de forma incremental (`001`, `002`, etc.) para manter a ordem de aplicação.

### 3. Criar Scripts de Migração em SQL

Crie arquivos de migração na pasta `migrations`. Cada migração deve conter os comandos SQL para modificar o banco, como criar, alterar ou remover tabelas.

#### Exemplo de uma Migração SQL

Para criar uma tabela `users`, crie o arquivo `001_create_users_table.sql`:

```sql
-- 001_create_users_table.sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```

Para adicionar uma coluna `email`, crie `002_add_email_to_users.sql`:

```sql
-- 002_add_email_to_users.sql
ALTER TABLE users ADD COLUMN email VARCHAR(255) UNIQUE;
```

### 4. Criar uma Tabela de Controle de Migrações

Para rastrear quais migrações foram aplicadas, crie uma tabela de controle no banco de dados:

```sql
CREATE TABLE IF NOT EXISTS migrations (
    id SERIAL PRIMARY KEY,
    migration_file VARCHAR(255) NOT NULL,
    applied_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
```

### 5. Criar o Script `migration-runner.js`

Este script em Node.js aplicará as migrações em ordem, verificando quais já foram executadas.

```javascript
// migration-runner.js
const { Client } = require('pg');
const fs = require('fs');
const path = require('path');

// Configuração de conexão ao PostgreSQL
const client = new Client({
  user: 'seu_usuario',
  host: 'localhost',
  database: 'nome_do_banco',
  password: 'sua_senha',
  port: 5432,
});

// Função para obter migrações aplicadas
async function getAppliedMigrations() {
  const res = await client.query('SELECT migration_file FROM migrations');
  return res.rows.map(row => row.migration_file);
}

// Função para aplicar uma única migração
async function applyMigration(file) {
  const sql = fs.readFileSync(path.join(__dirname, 'migrations', file), 'utf8');
  await client.query(sql);
  await client.query('INSERT INTO migrations (migration_file) VALUES ($1)', [file]);
}

// Função principal para rodar todas as migrações
async function runMigrations() {
  await client.connect();
  
  await client.query(`CREATE TABLE IF NOT EXISTS migrations (
    id SERIAL PRIMARY KEY,
    migration_file VARCHAR(255) NOT NULL,
    applied_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
  )`);

  const appliedMigrations = await getAppliedMigrations();
  const migrationFiles = fs.readdirSync(path.join(__dirname, 'migrations')).filter(
    file => !appliedMigrations.includes(file)
  );

  for (const file of migrationFiles) {
    console.log(`Aplicando migração: ${file}`);
    await applyMigration(file);
  }

  await client.end();
  console.log('Migrações aplicadas com sucesso.');
}

// Executa as migrações
runMigrations().catch(error => console.error('Erro ao aplicar migrações:', error));
```

### 6. Executar as Migrações

No terminal, execute o script de migração:

```bash
node migration-runner.js
```

Esse script fará o seguinte:
- Conectará ao banco de dados PostgreSQL.
- Verificará a tabela `migrations` para ver quais migrações já foram aplicadas.
- Aplicará as migrações pendentes em ordem e registrará cada migração aplicada na tabela `migrations`.

### 7. Reverter uma Migração (Manual)

Como estamos usando SQL puro, o rollback (reversão) não é automático. Para reverter uma migração, crie um arquivo SQL separado para desfazer a alteração ou edite o script da migração, e execute manualmente:

Exemplo de rollback para `002_add_email_to_users.sql`:

```sql
-- 002_remove_email_from_users.sql (exemplo de rollback)
ALTER TABLE users DROP COLUMN IF EXISTS email;
```

### 8. Executar Rollbacks (Opcional)

Se deseja implementar rollbacks de forma automatizada, você pode:
1. Criar arquivos `down` para cada migração (e.g., `002_add_email_to_users_down.sql`).
2. Adaptar o `migration-runner.js` para buscar e executar esses arquivos na ordem inversa.

## Considerações Finais

- **Backup do banco de dados**: faça sempre backups antes de aplicar migrações em produção.
- **Controle de versão**: versionar os arquivos de migração no Git ajuda a manter um histórico claro de alterações no banco de dados.
- **Automação**: configure o script `migration-runner.js` para rodar durante o deploy, aplicando automaticamente as migrações mais recentes.

Com esse método, você pode gerenciar o esquema de banco de dados de forma eficiente, sem depender de ORMs, mantendo o controle completo sobre cada alteração no PostgreSQL.
```
