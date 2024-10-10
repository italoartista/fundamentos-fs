# Principais conceitos em Bancos de Dados

## 1. **Banco de Dados (Database)**

Um banco de dados é uma coleção organizada de dados, geralmente armazenada e acessada eletronicamente. Pode ser estruturado em tabelas, colunas e linhas.

### Diagrama:

```markdown
+-----------------------+
|      Banco de Dados   |
+-----------------------+
|  Tabela 1             |
|  Tabela 2             |
|  Tabela 3             |
+-----------------------+
```

## 2. **Tabela (Table)**

Uma tabela é uma estrutura que armazena dados em linhas e colunas. Cada linha é um registro, e cada coluna é um campo do registro.

### Diagrama:

```markdown
+-------------------+
|  Tabela: Usuarios |
+-------------------+
| ID  | Nome | Idade |
|-----|------|-------|
| 1   | Ana  | 30    |
| 2   | João | 25    |
+-------------------+
```

## 3. **Registro (Record)**

Um registro é uma linha em uma tabela. Ele representa uma entidade completa, como um usuário ou um produto.

### Exemplo:

```markdown
| ID  | Nome | Idade |
|-----|------|-------|
| 1   | Ana  | 30    |
```

## 4. **Campo (Field)**

Um campo é uma coluna em uma tabela. Cada campo armazena um tipo específico de dado, como texto, número ou data.

### Exemplo:

```markdown
| Nome  |
|-------|
| Ana   |
| João  |
```

## 5. **Chave Primária (Primary Key)**

A chave primária é um campo ou conjunto de campos que identifica exclusivamente cada registro em uma tabela. Não pode haver valores duplicados ou nulos.

### Diagrama:

```markdown
+-------------------+
|  Tabela: Usuarios |
+-------------------+
| **ID** | Nome | Idade |
|--------|------|-------|
| 1      | Ana  | 30    |
| 2      | João | 25    |
+-------------------+
```

## 6. **Chave Estrangeira (Foreign Key)**

A chave estrangeira é um campo em uma tabela que cria um relacionamento com a chave primária de outra tabela. Ela ajuda a manter a integridade referencial entre tabelas.

### Diagrama:

```markdown
+-------------------+       +-------------------+
|  Tabela: Pedidos  |       |  Tabela: Usuarios |
+-------------------+       +-------------------+
| ID  | ID_Usuario |       | **ID** | Nome  |
|-----|------------|       |--------|-------|
| 1   | 1          |       | 1      | Ana   |
| 2   | 2          |       | 2      | João  |
+-------------------+       +-------------------+
```

## 7. **Índice (Index)**

Um índice é uma estrutura que melhora a velocidade das operações de consulta em uma tabela. Ele cria um acesso mais rápido aos dados, similar a um índice em um livro.

### Diagrama:

```markdown
+-------------------+
|  Tabela: Usuarios |
+-------------------+
| **ID** | Nome | Idade |
|--------|------|-------|
| 1      | Ana  | 30    |
| 2      | João | 25    |
+-------------------+
    ^
    |
  Índice
```

## 8. **Normalização (Normalization)**

Normalização é o processo de organizar os dados para reduzir a redundância e melhorar a integridade dos dados. Envolve a divisão de grandes tabelas em tabelas menores e a definição de relacionamentos entre elas.

### Exemplo de Normalização:

```markdown
+---------------------+
|  Tabela: Produtos   |
+---------------------+
| **ID_Produto** | Nome      |
|----------------|-----------|
| 1              | Produto A |
| 2              | Produto B |
+---------------------+

+--------------------+
|  Tabela: Categorias |
+--------------------+
| **ID_Categoria** | Categoria |
|------------------|-----------|
| 1                | Categoria 1|
| 2                | Categoria 2|
+--------------------+
```

## 9. **SQL (Structured Query Language)**

SQL é a linguagem padrão usada para interagir com bancos de dados relacionais. Com SQL, você pode consultar, inserir, atualizar e excluir dados.

### Exemplos:

- **Consulta**:

  ```sql
  SELECT Nome, Idade FROM Usuários WHERE Idade > 25;
  ```

- **Inserção**:

  ```sql
  INSERT INTO Usuarios (Nome, Idade) VALUES ('Carlos', 28);
  ```

## 10. **Transação (Transaction)**

Uma transação é um conjunto de operações que são executadas como uma única unidade. As transações devem ser atômicas, consistentes, isoladas e duráveis (propriedades ACID).

### Exemplo de Transação:

```sql
BEGIN TRANSACTION;

UPDATE Conta SET Saldo = Saldo - 100 WHERE ID = 1;
UPDATE Conta SET Saldo = Saldo + 100 WHERE ID = 2;

COMMIT;
```
