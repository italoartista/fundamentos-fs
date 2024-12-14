### Diferença entre Arquitetura em Camadas e MVC

**1. Arquitetura em Camadas**
- **Definição**: A arquitetura em camadas organiza o sistema em camadas separadas, onde cada camada tem uma responsabilidade clara e se comunica apenas com a camada adjacente. Geralmente, temos:
  - **Camada de Apresentação (UI)**: Interface do usuário.
  - **Camada de Aplicação**: Lida com lógica de negócios específica do caso de uso.
  - **Camada de Domínio**: Contém regras de negócios puras e independentes.
  - **Camada de Infraestrutura**: Comunicação com bancos de dados, APIs, arquivos etc.

- **Exemplo em TypeScript**:
```typescript
// Camada de Infraestrutura
class UserRepository {
  findById(id: string): User {
    // Acessa o banco de dados
    return new User(id, "John Doe");
  }
}

// Camada de Domínio
class User {
  constructor(public id: string, public name: string) {}
  greet(): string {
    return `Hello, ${this.name}!`;
  }
}

// Camada de Aplicação
class UserService {
  constructor(private userRepository: UserRepository) {}
  getUserGreeting(id: string): string {
    const user = this.userRepository.findById(id);
    return user.greet();
  }
}

// Camada de Apresentação
const userService = new UserService(new UserRepository());
console.log(userService.getUserGreeting("1")); // "Hello, John Doe!"
```

**2. MVC (Model-View-Controller)**
- **Definição**: É um padrão de design que divide a aplicação em três partes:
  - **Model**: Gerencia dados, lógica de negócios e regras de aplicação.
  - **View**: Representa a interface com o usuário.
  - **Controller**: Lida com requisições do usuário e chama métodos no Model e View.

- **Exemplo em TypeScript**:
```typescript
// Model
class Product {
  constructor(public id: string, public name: string, public price: number) {}
}

// View
class ProductView {
  render(product: Product): void {
    console.log(`Product: ${product.name} - $${product.price}`);
  }
}

// Controller
class ProductController {
  constructor(private productView: ProductView) {}
  displayProduct(): void {
    const product = new Product("1", "Laptop", 1500);
    this.productView.render(product);
  }
}

const view = new ProductView();
const controller = new ProductController(view);
controller.displayProduct(); // Output: Product: Laptop - $1500
```

---

### Diferenças Principais
| **Aspecto**            | **Arquitetura em Camadas**                       | **MVC**                          |
|-------------------------|------------------------------------------------|----------------------------------|
| **Estrutura**           | Baseada em camadas separadas.                  | Baseada em três componentes (MVC). |
| **Comunicação**         | Comunicação clara entre camadas adjacentes.    | Comunicação direta entre Model, View e Controller. |
| **Escopo**              | Design de arquitetura completa.               | Padrão de design de componentes. |
| **Flexibilidade**       | Mais flexível para grandes sistemas.           | Ideal para sistemas menores.    |

---

### Variações de MVC

1. **MVVM (Model-View-ViewModel)**  
   - **Definição**: O **ViewModel** atua como um intermediário, expondo dados e manipulando a lógica de interação.
   - **Exemplo**:
     ```typescript
     class ProductViewModel {
       product: Product;
       constructor() {
         this.product = new Product("1", "Tablet", 500);
       }
       get productInfo(): string {
         return `${this.product.name} - $${this.product.price}`;
       }
     }

     const viewModel = new ProductViewModel();
     console.log(viewModel.productInfo); // "Tablet - $500"
     ```

2. **MVP (Model-View-Presenter)**  
   - **Definição**: O **Presenter** contém toda a lógica de apresentação e interage diretamente com o Model e a View.
   - **Exemplo**:
     ```typescript
     class ProductPresenter {
       constructor(private product: Product, private view: ProductView) {}
       showProduct(): void {
         this.view.render(this.product);
       }
     }

     const product = new Product("2", "Smartphone", 800);
     const view = new ProductView();
     const presenter = new ProductPresenter(product, view);
     presenter.showProduct(); // "Smartphone - $800"
     ```

3. **PAC (Presentation-Abstraction-Control)**  
   - **Definição**: Componentes divididos em **Apresentação**, **Abstração**, e **Controle**, geralmente usado para sistemas distribuídos.
   - **Exemplo**: Estruturas como Microfrontends usam variantes desse padrão.

### Conclusão
- **Arquitetura em camadas** é mais genérica e aplicada em sistemas complexos e escaláveis.
- **MVC e suas variações** são padrões mais específicos, ideais para organizar a lógica de UI e componentes. Ambos podem ser usados em conjunto.

### Diferenças Detalhadas: Arquitetura em Camadas vs. MVC - Revisão

**Arquitetura em Camadas** e **MVC** não são mutuamente exclusivos; muitas vezes, sistemas grandes integram ambos. Vamos expandir o entendimento com:

- **Conceitos Chave**.
- **Prós e Contras**.
- **Exemplos avançados**.

---

### **1. Arquitetura em Camadas**

#### Conceitos Chave
1. **Separação Estrita de Responsabilidades**:
   Cada camada tem uma responsabilidade clara:
   - **Apresentação (UI)**: Interface do usuário (web, desktop, mobile).
   - **Aplicação**: Orquestra fluxos de negócio.
   - **Domínio**: Contém as regras de negócio puras.
   - **Infraestrutura**: Faz o trabalho "sujo" como interagir com APIs, banco de dados, arquivos etc.

2. **Comunicação entre Camadas**:
   - Segue o princípio **unidirecional**. Por exemplo, a **UI** nunca acessa diretamente a **Infraestrutura**.

#### Vantagens
- **Manutenção**: Alterar uma camada geralmente não afeta outras.
- **Escalabilidade**: Adicionar novas funcionalidades não exige retrabalho em todas as camadas.
- **Testabilidade**: Camadas são independentes e podem ser testadas isoladamente.

#### Desvantagens
- **Overhead Inicial**: Adicionar camadas pode aumentar a complexidade para sistemas simples.
- **Latência**: Comunicação entre camadas pode reduzir a performance.

#### Exemplo Avançado com TypeScript
Um sistema de e-commerce com persistência no banco de dados, regras de desconto e interface RESTful.

```typescript
// Camada de Infraestrutura
class ProductRepository {
  private products = [
    { id: "1", name: "Laptop", price: 1500 },
    { id: "2", name: "Smartphone", price: 800 },
  ];

  findById(id: string) {
    return this.products.find((p) => p.id === id) || null;
  }
}

// Camada de Domínio
class Product {
  constructor(public id: string, public name: string, public price: number) {}

  applyDiscount(percent: number): void {
    this.price = this.price - (this.price * percent) / 100;
  }
}

// Camada de Aplicação
class ProductService {
  constructor(private repository: ProductRepository) {}

  getProductWithDiscount(id: string, discount: number): Product | null {
    const productData = this.repository.findById(id);
    if (!productData) return null;
    const product = new Product(productData.id, productData.name, productData.price);
    product.applyDiscount(discount);
    return product;
  }
}

// Camada de Apresentação (REST Controller)
const productService = new ProductService(new ProductRepository());
const product = productService.getProductWithDiscount("1", 10);
console.log(product); // { id: "1", name: "Laptop", price: 1350 }
```

---

### **2. MVC (Model-View-Controller)**

#### Conceitos Chave
1. **Componentes do MVC**:
   - **Model**:
     - Representa os dados e a lógica de negócios.
     - Não depende da View ou do Controller.
   - **View**:
     - Apresenta os dados ao usuário.
     - Observa o **Model** e atualiza dinamicamente quando os dados mudam.
   - **Controller**:
     - Recebe entrada do usuário, processa e delega ao Model ou View.

2. **Fluxo de Dados**:
   - **User Input → Controller → Model**.
   - **Model Update → View Update**.

#### Vantagens
- **Separação de Preocupações**: Componentes claramente definidos.
- **Reatividade**: Ideal para interfaces dinâmicas (com frameworks modernos).

#### Desvantagens
- **Dependências Complexas**: Em projetos maiores, a interação entre componentes pode complicar a manutenção.
- **Menos Modular**: Comparado à arquitetura em camadas.

#### Exemplo Avançado com TypeScript
Um sistema de tarefas com eventos de input do usuário.

```typescript
// Model
class Task {
  constructor(public id: string, public name: string, public completed: boolean = false) {}
}

class TaskModel {
  private tasks: Task[] = [];

  addTask(name: string) {
    const newTask = new Task(`${this.tasks.length + 1}`, name);
    this.tasks.push(newTask);
  }

  toggleTask(id: string) {
    const task = this.tasks.find((task) => task.id === id);
    if (task) task.completed = !task.completed;
  }

  getTasks(): Task[] {
    return this.tasks;
  }
}

// View
class TaskView {
  render(tasks: Task[]): void {
    console.clear();
    tasks.forEach((task) => {
      console.log(`[${task.completed ? "X" : " "}] ${task.id}: ${task.name}`);
    });
  }
}

// Controller
class TaskController {
  constructor(private model: TaskModel, private view: TaskView) {}

  addTask(name: string) {
    this.model.addTask(name);
    this.updateView();
  }

  toggleTask(id: string) {
    this.model.toggleTask(id);
    this.updateView();
  }

  updateView() {
    const tasks = this.model.getTasks();
    this.view.render(tasks);
  }
}

// Simulação
const model = new TaskModel();
const view = new TaskView();
const controller = new TaskController(model, view);

controller.addTask("Learn TypeScript");
controller.addTask("Build an MVC App");
controller.toggleTask("1");
```

---

### **Variações de MVC (Detalhadas)**

1. **MVVM (Model-View-ViewModel)**:
   - A **ViewModel** encapsula o estado e lógica da interface, expondo propriedades observáveis à **View**.
   - Exemplos:
     - **React com Hooks**.
     - **Angular com Two-Way Binding**.

2. **MVP (Model-View-Presenter)**:
   - O **Presenter** é responsável por toda a lógica de apresentação, enquanto a View se concentra apenas em renderizar.

3. **HMVC (Hierarchical MVC)**:
   - Usado em sistemas grandes, onde vários MVCs menores são organizados hierarquicamente.

---

### **Conclusão**

| **Aspecto**              | **Arquitetura em Camadas**                                  | **MVC**                                   |
|--------------------------|-----------------------------------------------------------|------------------------------------------|
| **Escopo**               | Estrutura de alto nível para sistemas grandes.            | Padrão focado na interação do usuário.   |
| **Modularidade**         | Alta: separação entre camadas.                            | Média: interação intensa entre MVC.      |
| **Usabilidade**          | Backend e lógica de negócios.                            | UI e fluxo de dados em interfaces.       |
| **Flexibilidade**        | Melhor para sistemas complexos.                          | Melhor para UIs dinâmicas.               |

Os dois podem ser combinados para um sistema mais robusto. Um exemplo comum é uma aplicação RESTful (em camadas) com front-end React (MVVM).
