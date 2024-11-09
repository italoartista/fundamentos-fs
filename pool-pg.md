O objeto `Pool` do PostgreSQL, quando configurado com o pacote `pg` em Node.js, possui diversas opções de configuração para gerenciar as conexões de forma eficiente. Abaixo estão as propriedades e métodos mais comuns que podem ser configurados no objeto `Pool`:

### Propriedades Configuráveis do `Pool`:

1. **`connectionString`** (string): 
   - A URL de conexão para o banco de dados (exemplo: `postgres://user:password@host:port/database`).
   - Usado para especificar como o cliente se conecta ao banco de dados.
   
2. **`max`** (número):
   - O número máximo de conexões simultâneas no pool. O padrão é 10.
   
3. **`min`** (número):
   - O número mínimo de conexões no pool. O padrão é 0, significando que não há conexões mínimas.

4. **`idleTimeoutMillis`** (número):
   - O tempo em milissegundos que uma conexão ociosa pode permanecer no pool antes de ser fechada. O padrão é 10000 (10 segundos).
   
5. **`connectionTimeoutMillis`** (número):
   - O tempo máximo em milissegundos para aguardar uma nova conexão ser estabelecida antes de gerar um erro. O padrão é 0 (sem limite).
   
6. **`statement_timeout`** (número):
   - O tempo máximo que uma consulta pode levar antes de ser cancelada, em milissegundos. Por exemplo, `30000` significa 30 segundos.
   
7. **`query_timeout`** (número):
   - O tempo máximo de execução para consultas em cada conexão do pool.

8. **`ssl`** (objeto ou booleano):
   - Se definido como `true`, ativa a conexão SSL. Pode também ser um objeto para configurações adicionais de SSL (como chaves e certificados).

9. **`host`** (string):
   - O endereço do servidor PostgreSQL (exemplo: `localhost` ou um endereço IP).

10. **`port`** (número):
    - A porta onde o PostgreSQL está rodando, geralmente `5432`.

11. **`user`** (string):
    - O nome do usuário para autenticação no PostgreSQL.

12. **`password`** (string):
    - A senha do usuário do PostgreSQL.

13. **`database`** (string):
    - O nome do banco de dados para o qual se está se conectando.

14. **`application_name`** (string):
    - Nome da aplicação que aparece nas consultas de logs do PostgreSQL.

15. **`password_encryption`** (boolean):
    - Ativa ou desativa a criptografia da senha ao se conectar com o banco de dados.

16. **`statement_timeout`** (número):
    - Define o tempo máximo de execução das consultas (em milissegundos).

### Métodos e Funções do `Pool`:

1. **`connect()`**:
   - Este método retorna uma promessa (Promise) que resolve com um objeto `client`. Ele cria uma nova conexão ou reutiliza uma conexão do pool.
   
   Exemplo:
   ```javascript
   const client = await pool.connect();
   ```

2. **`end()`**:
   - Este método é utilizado para liberar todos os recursos do pool e fechar todas as conexões.
   
   Exemplo:
   ```javascript
   await pool.end();
   ```

3. **`query()`**:
   - Esse método é utilizado para executar uma consulta SQL diretamente no pool de conexões.
   
   Exemplo:
   ```javascript
   const result = await pool.query('SELECT * FROM users');
   ```

4. **`on(event, callback)`**:
   - O pool também emite eventos como `connect`, `remove`, `error`, etc. É possível escutar esses eventos para customizar comportamentos do pool.
   
   Exemplo:
   ```javascript
   pool.on('error', (err, client) => {
     console.error('Error in idle client', err.message, err.stack);
   });
   ```

### Exemplo Completo de Configuração do Pool:

```javascript
const { Pool } = require('pg');

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 20,  // Máximo de 20 conexões simultâneas
  min: 5,   // Mínimo de 5 conexões ativas no pool
  idleTimeoutMillis: 30000,  // Tempo de 30 segundos para fechar conexões ociosas
  connectionTimeoutMillis: 2000,  // Tempo máximo de 2 segundos para criar uma nova conexão
  ssl: {
    rejectUnauthorized: false  // Se necessário configurar SSL
  },
  application_name: 'meu_app',
  query_timeout: 5000  // Timeout de 5 segundos para consultas
});

// Usando o pool para conectar ao banco de dados
async function fetchUsers() {
  const client = await pool.connect();
  try {
    const res = await client.query('SELECT * FROM users');
    console.log(res.rows);
  } finally {
    client.release();  // Libera o client para o pool
  }
}
```

### Conclusão

Essas são algumas das principais propriedades e métodos que você pode usar para configurar e trabalhar com o pool de conexões do `pg` em Node.js. Dependendo do seu caso de uso, você pode ajustar essas configurações para otimizar o desempenho e garantir a confiabilidade da conexão com o banco de dados PostgreSQL.
