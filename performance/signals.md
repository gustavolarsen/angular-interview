# O que é e como funciona o Signals no Angular?

O Signal é um recurso novo do Angular que iniciou na v16 e cegou com mais força na v17 e que oferece uma maneira mais eficiente de gerenciar o estado das suas aplicações. Ele se baseia no conceito de **reactive programming**, permitindo que você construa interfaces de usuário reativas e otimizadas.

**Para que serve o Signal?**

* **Gerenciar estado:** Sinaliza alterações no estado da sua aplicação. Você pode acompanhar as modificações e atualizar a interface de usuário de acordo.
* **Reação a eventos:** Reage a eventos externos, como cliques de botões, entradas de formulários, etc., e atualiza o estado de forma eficiente.
* **Gerenciar dados assíncronos:** Simplifica a manipulação de dados assíncronos, como chamadas de API, garantindo uma atualização suave da interface.
* **Melhorar o desempenho:** Reduz a necessidade de re-renderizações desnecessárias, otimizando o desempenho da aplicação.

**Como utilizar Signal no Angular 17:**

1. **Instalação:** Ative o recurso `standalone` no seu projeto usando a versão mais recente do Angular.
2. **Exemplo de um Signal:**

    ```typescript
    import { Component, signal } from '@angular/core';

    @Component({
      selector: 'app-my-component',
      standalone: true,
      template: `
        <p>Contagem: {{ count() }}</p>
        <button (click)="increment()">Incrementar</button>
      `
    })
    export class MyComponent {
      count = signal(0);

      increment() {
        this.count.set(this.count() + 1);
      }
    }
    ```

    * `signal(0)`: Define um Signal chamado `count` com o valor inicial 0.
    * `count()`: Acessa o valor atual do Signal `count`.
    * `count.set(valor)`: Define um novo valor para o Signal `count`.

3. **Usando Signal no template:**

    * Utilize a sintaxe `{{ count() }}` para exibir o valor do Signal no template.
    * Ajuste o template de acordo com o seu código.

4. **Criando um Signal assíncrono:**

    ```typescript
    import { Component, signal, effect } from '@angular/core';

    @Component({
      selector: 'app-my-component',
      standalone: true,
      template: `
        <p *ngIf="isLoading()">Carregando...</p>
        <p *ngIf="!isLoading()">Dados: {{ data() }}</p>
      `
    })
    export class MyComponent {
      isLoading = signal(true);
      data = signal<string | null>(null);

      ngOnInit() {
        effect(() => {
          fetch('https://api.example.com/data')
            .then(res => res.json())
            .then(data => {
              this.data.set(data);
              this.isLoading.set(false);
            });
        });
      }
    }
    ```

    * `effect()`: Define um efeito que é executado quando o Signal é atualizado.
    * `isLoading.set(false)`: Define o Signal `isLoading` como falso ao receber os dados.

**Benefícios de usar Signal:**

* **Código limpo e organizado:** Código mais legível e fácil de entender.
* **Performance aprimorada:** A interface de usuário atualiza apenas quando necessário.
* **Depuração simplificada:** A depuração do código é mais fácil, com o histórico de alterações do estado.

# Como manipular um Signal através dos métodos set(), update(), mutate() e computed()?

O Angular 17 introduz os Signals como uma nova maneira de gerenciar o estado da aplicação. Esses Signals possuem métodos específicos para manipular e interagir com seus valores. Vamos analisar os quatro métodos principais:

**1. set():**

* **Objetivo:** Define um novo valor para o Signal. 
* **Uso:** `signal.set(novoValor)`
* **Exemplo:**
    ```typescript
    let count = signal(0);
    count.set(5); // Agora, count tem o valor 5.
    ```

**2. update():**

* **Objetivo:** Atualiza o valor do Signal usando a função fornecida como argumento.
* **Uso:** `signal.update((valorAtual) => novoValor)`
* **Exemplo:**
    ```typescript
    let count = signal(0);
    count.update(valor => valor + 1); // Incrementa o valor de count em 1.
    ```
    * O valor atual do Signal é passado para a função como argumento. 
    * O valor retornado pela função é usado para atualizar o Signal.

**3. mutate():**

* **Objetivo:** Modifica diretamente o valor do Signal sem criar uma nova instância.
* **Uso:** `signal.mutate((valorAtual) => { valorAtual.propriedade = novoValor; })`
* **Exemplo:**
    ```typescript
    let user = signal({ nome: 'João', idade: 30 });
    user.mutate(user => { user.nome = 'Maria'; }); // Altera o nome do objeto user.
    ```
    * Ideal para objetos e arrays.
    * A função passada como argumento recebe o valor atual do Signal como referência.
    * As modificações dentro da função são aplicadas diretamente ao valor original do Signal.

**4. computed():**

* **Objetivo:** Cria um Signal derivado de outro Signal, calculando seu valor com base no valor do Signal original.
* **Uso:** `computed(() => funcaoCalculo(signalOriginal()))`
* **Exemplo:**
    ```typescript
    let count = signal(0);
    let doubledCount = computed(() => count() * 2); // Calcula o dobro do valor de count.
    ```
    * O Signal `doubledCount` é calculado automaticamente quando o valor de `count` muda.
    * É ideal para derivar novos valores ou realizar cálculos complexos com base em outros Signals.

**Diferenças entre set() e update():**

* **set()** substitui o valor atual do Signal completamente. 
* **update()** utiliza o valor atual como base para a atualização, permitindo realizar operações como incrementos, decrementos ou outras modificações.

**Importante:**

* Os métodos `set()`, `update()` e `mutate()` alteram o valor do Signal e provocam uma re-renderização dos componentes que dependem desse Signal.
* O método `computed()` não altera o valor do Signal original, mas cria um novo Signal baseado em seu valor.

Para aprender mais, consulte a documentação oficial do Angular: [https://angular.io/guide/signals](https://angular.io/guide/signals).
