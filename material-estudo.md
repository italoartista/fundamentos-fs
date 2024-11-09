# **Material educacional: integração de apis com banco de dados usando node.js e postgresql**

## **Objetivo**

Neste material, abordaremos como integrar **APIs** com **banco de dados** em uma aplicação construída com **node.js** e **PostgreSQL**. Através de exemplos práticos, vamos explorar como criar **endpoints RESTful** para operações **CRUD**, como implementar **transações** para garantir a integridade dos dados e como seguir as melhores práticas de **segurança** e **escabilidade** ao construir uma API robusta e segura.

## **Conteúdo programático**

### **1. Introdução às APIs RESTful**

Uma **API RESTful** (Representational State Transfer) é um estilo arquitetônico para sistemas distribuídos que permite que diferentes sistemas se comuniquem entre si de maneira simples e escalável. A principal característica das APIs RESTful é que elas são baseadas no protocolo HTTP e utilizam seus métodos para realizar operações em recursos (dados).

#### **Princípios das APIs RESTful**
- **Stateless (Sem Estado)**: Cada requisição feita à API deve conter todas as informações necessárias para que a operação seja realizada, sem depender de informações de requisições anteriores.
- **Cacheable (Armazenamento em cache)**: As respostas da API podem ser armazenadas em cache para melhorar a performance.
- **Interface uniforme**: A API deve ser consistente e os métodos devem ser padronizados para facilitar a integração e a manutenção.

#### **Verbos HTTP**
- **GET**: Recupera recursos (dados) do servidor.
- **POST**: Cria um novo recurso.
- **PUT**: Atualiza um recurso existente.
- **DELETE**: Remove um recurso.
- **PATCH**: Atualiza parcialmente um recurso.

#### **Exemplo de boas práticas em APIs RESTful**
- **URLs claras e semânticas**:
  - `/items`: Lista de itens
  - `/items/{id}`: Item específico
  - `/orders`: Criação de um pedido
  - `/orders/{id}/items`: Itens de um pedido específico

- **Uso de códigos de status HTTP**:
  - **200 OK**: A requisição foi bem-sucedida.
  - **201 Created**: Um novo recurso foi criado.
  - **400 Bad Request**: A requisição é inválida.
  - **404 Not Found**: O recurso não foi encontrado.
  - **500 Internal Server Error**: Erro interno no servidor.

---

### **2. Conectando Node.js ao PostgreSQL**

Para interagir com o banco de dados **PostgreSQL**, utilizaremos a biblioteca **pg** (node-postgres), que é uma das formas mais comuns de realizar essa integração com o Node.js. Além disso, falaremos sobre a importância da configuração de uma **pool de conexões**, que é uma técnica para melhorar a performance do acesso ao banco de dados, gerenciando múltiplas conexões simultâneas.

#### **Configuração da Pool de Conexões**

O **Pool** é responsável por gerenciar um conjunto de conexões com o banco de dados, reutilizando-as quando necessário, ao invés de abrir uma nova conexão para cada requisição. Isso melhora a performance e evita o esgotamento de recursos.

```javascript
const { Pool } = require('pg');
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 10, // Máximo de conexões simultâneas
  idleTimeoutMillis: 30000, // Tempo de espera antes de fechar uma conexão ociosa
  connectionTimeoutMillis: 2000, // Tempo de espera para uma nova conexão
});
```

- **`max`**: Número máximo de conexões simultâneas que a pool pode manter.
- **`idleTimeoutMillis`**: Tempo de espera antes de fechar uma conexão ociosa.
- **`connectionTimeoutMillis`**: Tempo de espera para estabelecer uma conexão com o banco de dados.

#### **Conexão e Execução de Consultas**

Ao utilizar o **Pool**, você pode conectar-se ao banco e realizar operações de forma eficiente, como consultas e manipulação de dados.

```javascript
async function getItems() {
  const client = await pool.connect();
  try {
    const result = await client.query('SELECT * FROM items');
    return result.rows;
  } catch (err) {
    console.error('Erro ao recuperar os itens:', err);
    throw err;
  } finally {
    client.release(); // Libera o cliente após a execução
  }
}
```

---

### **3. Criando Endpoints RESTful**

Agora, vamos aprender a criar os **endpoints RESTful** para realizar operações **CRUD** (Criar, Ler, Atualizar, Deletar) no banco de dados. Vamos implementar um sistema simples de gestão de **itens**.

#### **Estrutura do Endpoint**

- **POST /items**: Cria um novo item.
- **GET /items**: Recupera todos os itens.
- **GET /items/:id**: Recupera um item específico.
- **PUT /items/:id**: Atualiza um item existente.
- **DELETE /items/:id**: Deleta um item.

```javascript
const express = require('express');
const app = express();
app.use(express.json());

// Endpoint para criar um novo item
app.post('/items', async (req, res) => {
  const { name, price } = req.body;
  try {
    const result = await pool.query(
      'INSERT INTO items (name, price) VALUES ($1, $2) RETURNING *',
      [name, price]
    );
    res.status(201).json(result.rows[0]);
  } catch (err) {
    console.error(err);
    res.status(500).send('Erro ao criar o item');
  }
});

// Endpoint para obter todos os itens
app.get('/items', async (req, res) => {
  try {
    const result = await pool.query('SELECT * FROM items');
    res.status(200).json(result.rows);
  } catch (err) {
    console.error(err);
    res.status(500).send('Erro ao recuperar os itens');
  }
});

// Endpoint para obter um item específico
app.get('/items/:id', async (req, res) => {
  const { id } = req.params;
  try {
    const result = await pool.query('SELECT * FROM items WHERE id = $1', [id]);
    if (result.rows.length === 0) {
      res.status(404).send('Item não encontrado');
    } else {
      res.status(200).json(result.rows[0]);
    }
  } catch (err) {
    console.error(err);
    res.status(500).send('Erro ao recuperar o item');
  }
});

// Endpoint para atualizar um item
app.put('/items/:id', async (req, res) => {
  const { id } = req.params;
  const { name, price } = req.body;
  try {
    const result = await pool.query(
      'UPDATE items SET name = $1, price = $2 WHERE id = $3 RETURNING *',
      [name, price, id]
    );
    if (result.rows.length === 0) {
      res.status(404).send('Item não encontrado');
    } else {
      res.status(200).json(result.rows[0]);
    }
  } catch (err) {
    console.error(err);
    res.status(500).send('Erro ao atualizar o item');
  }
});

// Endpoint para deletar um item
app.delete('/items/:id', async (req, res) => {
  const { id } = req.params;
  try {
    const result = await pool.query('DELETE FROM items WHERE id = $1 RETURNING *', [id]);
    if (result.rows.length === 0) {
      res.status(404).send('Item não encontrado');
    } else {
      res.status(200).send('Item deletado com sucesso');
    }
  } catch (err) {
    console.error(err);
    res.status(500).send('Erro ao deletar o item');
  }
});
```

---

### **4. Implementando Transações**

Quando você precisa garantir que múltiplas operações no banco de dados sejam realizadas com sucesso, você deve utilizar **transações**. Uma transação permite agrupar várias consultas SQL em uma única unidade de trabalho. Se uma das operações falhar, todas as outras podem ser revertidas, garantindo a **atomicidade**.

#### **Exemplo de Transação**

Vamos criar um endpoint para realizar um pedido, que envolve várias operações de banco de dados (criar o pedido e adicionar itens ao pedido).

```javascript
app.post('/order', async (req, res) => {
  const { customerId, items } = req.body;
  const client = await pool.connect();

  try {
    await client.query('BEGIN'); // Inicia a transação

    // Cria o pedido
    const orderResult = await client.query(
      'INSERT INTO orders (customer_id) VALUES ($1) RETURNING id',
      [customerId]
    );
    const orderId = orderResult.rows[0].id;

    // Adiciona os itens ao pedido
    for (let item of items) {
      await client.query(
        'INSERT INTO order_items (order_id, item_id, quantity) VALUES ($1, $2, $3)',
        [orderId,

 item.itemId, item.quantity]
      );
    }

    await client.query('COMMIT'); // Confirma a transação
    res.status(201).json({ orderId });
  } catch (err) {
    await client.query('ROLLBACK'); // Desfaz a transação
    console.error(err);
    res.status(500).send('Erro ao criar o pedido');
  } finally {
    client.release();
  }
});
```

- **BEGIN**: Inicia a transação.
- **COMMIT**: Confirma todas as alterações feitas na transação.
- **ROLLBACK**: Reverte todas as alterações feitas na transação.

---

### **5. Segurança**

Ao construir uma API, a segurança é uma das maiores preocupações. Aqui estão algumas práticas para garantir a segurança da sua API:

- **Autenticação e Autorização**: Utilize **JWT** (JSON Web Tokens) para autenticação e autorização. Certifique-se de validar os tokens nas rotas protegidas.
- **Validação de Entrada**: Valide todas as entradas do usuário para prevenir **SQL Injection**, **Cross-Site Scripting (XSS)**, e outras vulnerabilidades.
- **HTTPS**: Sempre use **HTTPS** para proteger a comunicação entre o cliente e a API.

#### **Exemplo de Validação de Entrada**

```javascript
const { check, validationResult } = require('express-validator');

app.post('/items', [
  check('name').isString().notEmpty(),
  check('price').isFloat({ gt: 0 }),
], async (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  // Lógica para criar o item
});
```

---

### **6. Escalabilidade e Desempenho**

Para garantir que sua API possa escalar à medida que o número de usuários e requisições aumenta, considere as seguintes práticas:

- **Cache**: Utilize **Redis** ou outro mecanismo de cache para armazenar respostas frequentemente solicitadas.
- **Balanceamento de Carga**: Configure múltiplos servidores para distribuir as requisições.
- **Monitoramento**: Utilize ferramentas de monitoramento como **Prometheus** e **Grafana** para acompanhar a saúde e o desempenho da aplicação.

---

### **Conclusão**

Integrar uma API com um banco de dados é uma habilidade fundamental para o desenvolvimento de software. Neste material, abordamos desde a criação de **endpoints RESTful**, passando pela **integração com PostgreSQL**, até a implementação de **transações** e **segurança**. Lembre-se de seguir as boas práticas para garantir a manutenção da qualidade e a escalabilidade da sua aplicação.

---

Esse material agora contém uma explicação mais completa, com detalhes sobre boas práticas e exemplos de código para implementar um sistema robusto. Caso precise de mais detalhes sobre algum tópico específico, sinta-se à vontade para perguntar!
