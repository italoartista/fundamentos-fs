

### **1. Testes Simples**  
1. Soma de dois números:  
   ```javascript
   test('soma dois números positivos', () => {
     expect(2 + 3).toBe(5);
   });
   ```  
2. Comparação de strings:  
   ```javascript
   test('verifica se duas strings são iguais', () => {
     expect('Jest').toBe('Jest');
   });
   ```  

---

### **2. Testes com Funções Simples**  
3. Multiplicação:  
   ```javascript
   function multiplica(a, b) {
     return a * b;
   }

   test('multiplica dois números', () => {
     expect(multiplica(2, 3)).toBe(6);
   });
   ```  
4. Verifica se um número é par:  
   ```javascript
   function ehPar(num) {
     return num % 2 === 0;
   }

   test('verifica se 4 é par', () => {
     expect(ehPar(4)).toBe(true);
   });
   ```  

---

### **3. Testes de Objetos**  
5. Verifica propriedades de um objeto:  
   ```javascript
   const usuario = { nome: 'João', idade: 25 };

   test('verifica nome do usuário', () => {
     expect(usuario.nome).toBe('João');
   });
   ```  
6. Objeto completo:  
   ```javascript
   const livro = { titulo: 'O Hobbit', autor: 'Tolkien' };

   test('valida o objeto livro', () => {
     expect(livro).toEqual({ titulo: 'O Hobbit', autor: 'Tolkien' });
   });
   ```  

---

### **4. Testes com Arrays**  
7. Contém um elemento:  
   ```javascript
   const frutas = ['maçã', 'banana', 'laranja'];

   test('verifica se contém banana', () => {
     expect(frutas).toContain('banana');
   });
   ```  
8. Verifica o tamanho:  
   ```javascript
   test('tamanho do array de frutas', () => {
     expect(frutas.length).toBe(3);
   });
   ```  

---

### **5. Testes com Funções Assíncronas**  
9. Simula uma API com `Promise`:  
   ```javascript
   function buscarUsuario() {
     return Promise.resolve({ nome: 'Maria' });
   }

   test('retorna nome do usuário', async () => {
     const data = await buscarUsuario();
     expect(data.nome).toBe('Maria');
   });
   ```  

---

### **6. Mocking**  
10. Mock de função para soma:  
   ```javascript
   const soma = jest.fn().mockReturnValue(10);

   test('mock de função soma', () => {
     expect(soma(2, 3)).toBe(10);
   });
   ```  

---

### **7. Testes de Erros**  
11. Verifica lançamento de erro:  
   ```javascript
   function dividir(a, b) {
     if (b === 0) throw new Error('Divisão por zero');
     return a / b;
   }

   test('lança erro para divisão por zero', () => {
     expect(() => dividir(4, 0)).toThrow('Divisão por zero');
   });
   ```  

---

### **8. Testes Funcionais**  
12. Calcula o preço com imposto:  
   ```javascript
   function calculaPreco(preco, imposto) {
     return preco + preco * imposto;
   }

   test('calcula preço final', () => {
     expect(calculaPreco(100, 0.2)).toBe(120);
   });
   ```  

---

### **9. Testes de Integração**  
13. Verifica o fluxo entre duas funções:  
   ```javascript
   function calcularFrete(distancia) {
     return distancia * 1.5;
   }

   function calcularTotal(valor, frete) {
     return valor + frete;
   }

   test('calcula total com frete', () => {
     const frete = calcularFrete(10);
     const total = calcularTotal(100, frete);
     expect(total).toBe(115);
   });
   ```  

---

### **10. Testes com Matchers Avançados**  
14. Verifica se o resultado é maior que um valor:  
   ```javascript
   test('verifica se resultado é maior que 10', () => {
     expect(15).toBeGreaterThan(10);
   });
   ```  
15. Verifica se é definido:  
   ```javascript
   const valor = 'Teste';

   test('valor está definido', () => {
     expect(valor).toBeDefined();
   });
   ```  

