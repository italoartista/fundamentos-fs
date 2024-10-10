# Exemplos Práticos de Propriedades ACID

## 1. Atomicidade

**Contexto:** Transferência de Dinheiro

**Descrição:** Suponha que você está transferindo $100 da Conta A para a Conta B.

**Transação:**
1. Débito de $100 da Conta A.
2. Crédito de $100 na Conta B.

**Exemplo:**

```sql
BEGIN TRANSACTION;

UPDATE contas
SET saldo = saldo - 100
WHERE conta_id = 'ContaA';

UPDATE contas
SET saldo = saldo + 100
WHERE conta_id = 'ContaB';

COMMIT;
```

**Cenário de Falha:** Se ocorrer um problema após o débito, mas antes do crédito, a atomicidade garante que nenhuma das operações seja realizada completamente. O banco de dados reverterá a transação e ambas as contas permanecerão inalteradas.

**Implementação:**
- Se o `COMMIT` falhar ou a transação for abortada, o banco de dados executará um `ROLLBACK`, revertendo todas as alterações realizadas até o momento.

## 2. Consistência

**Contexto:** Regras de Integridade de Dados

**Descrição:** Você tem uma tabela de "Clientes" com uma restrição de chave estrangeira que vincula os clientes a um "Estado".

**Tabela:**

```sql
CREATE TABLE Estados (
    estado_id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE Clientes (
    cliente_id INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    estado_id INT,
    FOREIGN KEY (estado_id) REFERENCES Estados(estado_id)
);
```

**Transação:**

```sql
BEGIN TRANSACTION;

-- Inserir um novo cliente com um estado existente
INSERT INTO Clientes (cliente_id, nome, estado_id)
VALUES (1, 'João', 1);

-- Tentar inserir um cliente com um estado que não existe
INSERT INTO Clientes (cliente_id, nome, estado_id)
VALUES (2, 'Maria', 99);

COMMIT;
```

**Cenário de Falha:** A transação falha na inserção do cliente com o `estado_id` 99, pois não há tal estado na tabela "Estados". A consistência garante que a tabela "Clientes" não contenha entradas inválidas.

**Implementação:**
- O banco de dados verifica a restrição de chave estrangeira e rejeita a inserção que violaria a integridade referencial.

## 3. Isolamento

**Contexto:** Leitura e Escrita Simultânea

**Descrição:** Dois usuários estão acessando o mesmo registro simultaneamente. Um usuário está atualizando o saldo da Conta X, e outro usuário está lendo esse saldo.

**Transações:**

- **Usuário 1 (Atualização):**

```sql
BEGIN TRANSACTION;

UPDATE contas
SET saldo = saldo - 50
WHERE conta_id = 'ContaX';

-- Não confirmar imediatamente
```

- **Usuário 2 (Leitura):**

```sql
BEGIN TRANSACTION;

SELECT saldo
FROM contas
WHERE conta_id = 'ContaX';

COMMIT;
```

**Cenário de Isolamento:** Dependendo do nível de isolamento configurado, o `SELECT` pode ver o saldo antigo ou o saldo atualizado. Com um nível de isolamento mais alto (como `SERIALIZABLE`), o `SELECT` esperará a conclusão da atualização do `Usuário 1`.

**Implementação:**
- O banco de dados usa bloqueios e mecanismos de controle de concorrência para garantir que a leitura não interfira nas atualizações simultâneas.

## 4. Durabilidade

**Contexto:** Persistência de Dados Após Falha

**Descrição:** Após uma transação bem-sucedida de inserção de um novo pedido em um sistema de e-commerce, você quer garantir que o pedido não seja perdido mesmo que o sistema falhe imediatamente após o `COMMIT`.

**Transação:**

```sql
BEGIN TRANSACTION;

INSERT INTO Pedidos (pedido_id, cliente_id, valor)
VALUES (1001, 123, 250.00);

COMMIT;
```

**Cenário de Falha:** Se o sistema falhar logo após o `COMMIT`, a durabilidade garante que o pedido será recuperado e persistido no banco de dados, não sendo perdido.

**Implementação:**
- O banco de dados utiliza logs de transação para registrar as alterações e pode recuperar o estado após falhas usando esses logs.
