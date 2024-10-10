# Conceitos ACID em Banco de Dados

## 1. Atomicidade

**Definição:** A atomicidade garante que uma transação no banco de dados seja tratada como uma unidade indivisível. Ou seja, uma transação deve ser totalmente concluída ou totalmente revertida.

**Detalhes:**
- **Completa ou Desfeita:** Se uma transação consiste em múltiplas operações, todas essas operações devem ser completadas com sucesso para que a transação seja considerada concluída. Caso contrário, todas as alterações realizadas devem ser revertidas.
- **Exemplo:** Considere uma transação que transfere dinheiro de uma conta para outra. Se a transferência debita uma conta mas falha ao creditar a outra, a atomicidade garante que ambas as operações sejam revertidas para manter a integridade dos dados.

**Implementação:** 
- **Commit e Rollback:** O banco de dados usa comandos `COMMIT` para finalizar a transação e `ROLLBACK` para reverter todas as alterações realizadas durante a transação.

## 2. Consistência

**Definição:** A consistência assegura que uma transação leva o banco de dados de um estado válido a outro estado válido, preservando todas as regras e restrições definidas no banco de dados.

**Detalhes:**
- **Regras e Restrições:** A consistência garante que todas as regras de integridade, como chaves primárias, chaves estrangeiras e outras restrições de dados, sejam respeitadas durante e após a execução da transação.
- **Exemplo:** Se uma transação altera uma tabela que contém restrições de integridade referencial, a consistência assegura que essas restrições não sejam violadas, e os dados permanecem corretos e válidos.

**Implementação:**
- **Verificação de Regras:** O sistema de gerenciamento de banco de dados (SGBD) aplica automaticamente as regras e restrições definidas ao executar uma transação.

## 3. Isolamento

**Definição:** O isolamento garante que as operações de uma transação sejam invisíveis para outras transações até que a transação seja completada. Isso evita que uma transação não finalizada afete outras transações simultâneas.

**Detalhes:**
- **Concorrência:** Isolamento permite que várias transações sejam realizadas simultaneamente sem interferir umas nas outras. Cada transação deve ser isolada para garantir que os resultados não sejam corrompidos por operações paralelas.
- **Níveis de Isolamento:** Existem diferentes níveis de isolamento, como Read Uncommitted, Read Committed, Repeatable Read e Serializable, que definem o grau de visibilidade e controle sobre as transações concorrentes.

**Implementação:**
- **Locks e Controle de Concorrência:** O banco de dados utiliza mecanismos de bloqueio e controle de concorrência para manter o isolamento entre transações.

## 4. Durabilidade

**Definição:** A durabilidade assegura que uma vez que uma transação é confirmada (commit), suas alterações no banco de dados são permanentes, mesmo em caso de falhas de sistema ou reinicializações.

**Detalhes:**
- **Persistência de Dados:** Após a confirmação de uma transação, seus efeitos são garantidos para persistir no banco de dados, independentemente de falhas de energia, falhas de hardware ou outros problemas imprevistos.
- **Exemplo:** Se um banco de dados confirma uma transação que adiciona um novo registro, esse registro deve permanecer no banco de dados e não ser perdido devido a uma falha.

**Implementação:**
- **Logs de Transação:** O banco de dados utiliza logs de transação (ou logs de recuperação) para registrar todas as alterações realizadas, permitindo que o sistema recupere o banco de dados para um estado consistente após uma falha.

---

Essas propriedades são fundamentais para garantir a integridade e confiabilidade das operações em um banco de dados, proporcionando uma base sólida para o desenvolvimento de aplicações que dependem de dados precisos e consistentes.
