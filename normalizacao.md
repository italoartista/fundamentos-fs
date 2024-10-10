
### Exemplo 1: Esquema Não Normalizado

Tabela **Pedidos**:

| PedidoID | ClienteNome | Produto        | Quantidade | Preço   | Data      | Endereço           |
|----------|-------------|----------------|------------|---------|-----------|--------------------|
| 101      | João Silva  | Teclado        | 2          | 50.00   | 2024-01-01| Rua A, 123, SP      |
| 102      | Maria Lima  | Notebook       | 1          | 2000.00 | 2024-01-02| Rua B, 456, RJ      |
| 103      | João Silva  | Monitor        | 1          | 500.00  | 2024-01-03| Rua A, 123, SP      |

Problemas:
- **Redundância de dados**: O endereço do cliente é repetido em várias linhas.
- **Atualizações complexas**: Se o endereço de João Silva mudar, será necessário atualizar todas as linhas onde ele aparece.
- **Inserção e exclusão problemática**: A tabela contém dados de produtos e de clientes misturados, dificultando a manutenção.

Agora, aplicaremos a normalização, começando pela Primeira Forma Normal (1NF).

---

## Primeira Forma Normal (1NF)

A **Primeira Forma Normal** elimina grupos repetidos e garante que todos os atributos contenham apenas valores atômicos.

### Passo 1: Remover grupos repetidos

| PedidoID | ClienteNome | Produto  | Quantidade | Preço   | Data      |
|----------|-------------|----------|------------|---------|-----------|
| 101      | João Silva  | Teclado  | 2          | 50.00   | 2024-01-01|
| 102      | Maria Lima  | Notebook | 1          | 2000.00 | 2024-01-02|
| 103      | João Silva  | Monitor  | 1          | 500.00  | 2024-01-03|

### Resultado:

- Removemos o endereço do cliente da tabela, mas o problema da redundância do nome e a repetição de informações sobre o cliente ainda existe.

---

## Segunda Forma Normal (2NF)

A **Segunda Forma Normal** elimina dependências parciais, ou seja, cada coluna deve depender completamente da chave primária.

### Passo 2: Criar tabelas separadas para Clientes e Produtos

**Clientes**:

| ClienteID | ClienteNome | Endereço         |
|-----------|-------------|------------------|
| 1         | João Silva  | Rua A, 123, SP   |
| 2         | Maria Lima  | Rua B, 456, RJ   |

**Produtos**:

| ProdutoID | Produto  | Preço   |
|-----------|----------|---------|
| 1         | Teclado  | 50.00   |
| 2         | Notebook | 2000.00 |
| 3         | Monitor  | 500.00  |

**Pedidos**:

| PedidoID | ClienteID | ProdutoID | Quantidade | Data      |
|----------|-----------|-----------|------------|-----------|
| 101      | 1         | 1         | 2          | 2024-01-01|
| 102      | 2         | 2         | 1          | 2024-01-02|
| 103      | 1         | 3         | 1          | 2024-01-03|

### Resultado:

- Agora os clientes e produtos são gerenciados separadamente, reduzindo a duplicação de dados.
- A tabela **Pedidos** faz referência a **ClienteID** e **ProdutoID**, relacionando clientes e produtos de maneira mais eficiente.

---

## Terceira Forma Normal (3NF)

A **Terceira Forma Normal** elimina dependências transitivas, ou seja, colunas que não sejam chave primária não devem depender de outras colunas não chave.

### Passo 3: Eliminar dependências transitivas

No exemplo anterior, todos os dados já estavam na 3NF, pois não há dependências transitivas. Vamos ver outro exemplo que será normalizado até a 3ª Forma Normal.

---

### Exemplo 2: Esquema Inicial

Tabela **Empregados**:

| EmpregadoID | Nome        | Departamento | Chefe     | Localização |
|-------------|-------------|--------------|-----------|-------------|
| 201         | Carlos      | TI           | João      | São Paulo   |
| 202         | Ana         | TI           | João      | São Paulo   |
| 203         | Pedro       | RH           | Maria     | Rio de Janeiro|

### Problemas:
- **Dependência transitiva**: O **Chefe** e a **Localização** dependem do **Departamento**, não do **EmpregadoID** diretamente.

---

### Aplicação da 3ª Forma Normal

**Empregados**:

| EmpregadoID | Nome   | DepartamentoID |
|-------------|--------|----------------|
| 201         | Carlos | 1              |
| 202         | Ana    | 1              |
| 203         | Pedro  | 2              |

**Departamentos**:

| DepartamentoID | Departamento | Chefe  | Localização   |
|----------------|--------------|--------|---------------|
| 1              | TI           | João   | São Paulo     |
| 2              | RH           | Maria  | Rio de Janeiro|

### Resultado:

- Agora, a tabela **Empregados** está normalizada, sem dependências transitivas, e a relação entre departamentos, chefes e localização foi movida para a tabela **Departamentos**.

---

## Exemplo Final: Até a 3ª Forma Normal

Para resumir, aqui está um exemplo final de uma tabela que foi normalizada de uma forma não normalizada até a Terceira Forma Normal.

### Esquema Inicial (Não Normalizado):

Tabela **Pedidos**:

| PedidoID | ClienteNome | Endereço           | Produto        | Quantidade | PreçoUnitário | Total    |
|----------|-------------|--------------------|----------------|------------|---------------|----------|
| 1        | João Silva  | Rua A, 123, SP     | Teclado        | 2          | 50.00         | 100.00   |
| 2        | Maria Lima  | Rua B, 456, RJ     | Monitor        | 1          | 500.00        | 500.00   |
| 3        | João Silva  | Rua A, 123, SP     | Notebook       | 1          | 2000.00       | 2000.00  |

### Esquema Normalizado (Até 3ª Forma Normal):

**Clientes**:

| ClienteID | Nome        | Endereço           |
|-----------|-------------|--------------------|
| 1         | João Silva  | Rua A, 123, SP     |
| 2         | Maria Lima  | Rua B, 456, RJ     |

**Produtos**:

| ProdutoID | Produto  | PreçoUnitário |
|-----------|----------|---------------|
| 1         | Teclado  | 50.00         |
| 2         | Monitor  | 500.00        |
| 3         | Notebook | 2000.00       |

**Pedidos**:

| PedidoID | ClienteID | ProdutoID | Quantidade | Total  |
|----------|-----------|-----------|------------|--------|
| 1        | 1         | 1         | 2          | 100.00 |
| 2        | 2         | 2         | 1          | 500.00 |
| 3        | 1         | 3         | 1          | 2000.00 |

Agora, as tabelas estão separadas por entidades (clientes, produtos e pedidos) e os dados estão relacionados de forma eficiente, sem redundância e com integridade.

---

Esses exemplos mostram como o processo de normalização pode ser aplicado para organizar dados de forma mais eficiente e manter a integridade e consistência no banco de dados.
