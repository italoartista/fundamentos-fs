```markdown
# Sintaxe Básica do SQL (PostgreSQL)

## 1. Comandos de Seleção (SELECT)

### 1.1 Selecionar Todos os Registros
```sql
SELECT * FROM nome_da_tabela;
```

### 1.2 Selecionar Colunas Específicas
```sql
SELECT coluna1, coluna2 FROM nome_da_tabela;
```

### 1.3 Filtrar Registros com WHERE
```sql
SELECT * FROM nome_da_tabela WHERE condição;
```

### 1.4 Ordenar Resultados
```sql
SELECT * FROM nome_da_tabela ORDER BY coluna ASC|DESC;
```

### 1.5 Agrupar Resultados
```sql
SELECT coluna, COUNT(*) FROM nome_da_tabela GROUP BY coluna;
```

## 2. Comandos de Inserção (INSERT)

### 2.1 Inserir um Registro
```sql
INSERT INTO nome_da_tabela (coluna1, coluna2) VALUES (valor1, valor2);
```

### 2.2 Inserir Múltiplos Registros
```sql
INSERT INTO nome_da_tabela (coluna1, coluna2) VALUES 
(valor1a, valor2a), 
(valor1b, valor2b);
```

## 3. Comandos de Atualização (UPDATE)

### 3.1 Atualizar Registros
```sql
UPDATE nome_da_tabela SET coluna1 = valor1 WHERE condição;
```

### 3.2 Atualizar Múltiplos Registros
```sql
UPDATE nome_da_tabela SET coluna1 = valor1 WHERE coluna2 = valor2;
```

## 4. Comandos de Exclusão (DELETE)

### 4.1 Deletar Registros
```sql
DELETE FROM nome_da_tabela WHERE condição;
```

### 4.2 Deletar Todos os Registros
```sql
DELETE FROM nome_da_tabela; -- Use com cautela!
```

## 5. Comandos de Alteração de Tabela (ALTER TABLE)

### 5.1 Adicionar uma Nova Coluna
```sql
ALTER TABLE nome_da_tabela ADD COLUMN nome_da_coluna tipo_de_dado [opções];
```

### 5.2 Alterar uma Coluna Existente
```sql
ALTER TABLE nome_da_tabela ALTER COLUMN nome_da_coluna SET NOT NULL;
```

### 5.3 Remover uma Coluna
```sql
ALTER TABLE nome_da_tabela DROP COLUMN nome_da_coluna;
```

## 6. Criação de Tabelas (CREATE TABLE)

### 6.1 Criar uma Nova Tabela
```sql
CREATE TABLE nome_da_tabela (
    coluna1 tipo_de_dado [opções],
    coluna2 tipo_de_dado [opções],
    ...
);
```

## 7. Chaves Primárias e Estrangeiras

### 7.1 Definir Chave Primária
```sql
CREATE TABLE nome_da_tabela (
    coluna1 tipo_de_dado PRIMARY KEY,
    ...
);
```

### 7.2 Definir Chave Estrangeira
```sql
CREATE TABLE nome_da_tabela (
    coluna1 tipo_de_dado,
    coluna2 tipo_de_dado,
    FOREIGN KEY (coluna1) REFERENCES outra_tabela(coluna);
);
```

## 8. Consultas Avançadas

### 8.1 JOIN (Junção de Tabelas)
```sql
SELECT * FROM tabela1 
JOIN tabela2 ON tabela1.coluna = tabela2.coluna;
```

### 8.2 LEFT JOIN
```sql
SELECT * FROM tabela1 
LEFT JOIN tabela2 ON tabela1.coluna = tabela2.coluna;
```

### 8.3 Subconsultas
```sql
SELECT * FROM tabela1 WHERE coluna IN (SELECT coluna FROM tabela2 WHERE condição);
```

## Conclusão

Esta é uma visão geral da sintaxe básica do SQL no PostgreSQL. Para operações mais complexas, consulte a documentação oficial ou pratique em um ambiente de banco de dados.
```

### Uso

Você pode copiar e colar este texto em um editor de Markdown, como o Typora, ou em plataformas que suportam Markdown, como GitHub, para visualizar a formatação.

Se precisar de mais informações ou alterações, é só avisar!
