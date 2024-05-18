# Como fazer a gestão de dependência do angular e qual o seu ciclo de vida?

No Angular, a gestão de dependências é feita principalmente através do sistema de injeção de dependência (Dependency Injection, DI). Este sistema permite que você injete serviços, que são classes que contêm lógica de negócios ou funcionalidade compartilhada, em componentes e outros serviços de maneira eficiente e testável.

### Gerenciamento de Dependências no Angular

1. **Provedores (Providers):**
   - **Root Provider:** Quando você declara um serviço no `@Injectable` com `{ providedIn: 'root' }`, ele é registrado no injetor raiz e terá um tempo de vida que dura enquanto o aplicativo estiver ativo.
   - **Module Provider:** Você pode declarar um serviço em um módulo específico, incluindo-o na lista de `providers` do módulo. Este serviço estará disponível para todos os componentes que pertencem a este módulo.
   - **Component Provider:** Um serviço declarado na lista de `providers` de um componente está disponível apenas para esse componente e seus componentes filhos.
  
   ```typescript
   @Injectable({
     providedIn: 'root',
   })
   export class MyService { ... }
   ```

   ```typescript
   @NgModule({
     providers: [MyService],
     ...
   })
   export class MyModule { }
   ```

   ```typescript
   @Component({
     providers: [MyService],
     ...
   })
   export class MyComponent { ... }
   ```

2. **Hierarquia de Injetores:**
   - **Injector Hierarchy:** Angular tem uma hierarquia de injetores, começando do `root` injector até os injetores específicos de componentes. Isso significa que um serviço pode ser compartilhado por todo o aplicativo ou apenas por partes específicas, dependendo de onde ele é declarado.

### Tempo de Vida da Injeção de Dependência

O tempo de vida de um serviço depende de onde ele é provido:

1. **Root Injector:**
   - **Singleton:** Um serviço fornecido no injetor raiz (`{ providedIn: 'root' }`) é um singleton e vive durante toda a vida útil do aplicativo. Este é o comportamento padrão para a maioria dos serviços em Angular.

2. **Module Injector:**
   - **Module Scope:** Se um serviço é declarado em um módulo (no `providers` de um módulo), ele será instanciado uma vez por módulo, e essa instância será compartilhada entre todos os componentes desse módulo.

3. **Component Injector:**
   - **Component Scope:** Quando um serviço é declarado no `providers` de um componente, uma nova instância do serviço é criada para cada instância do componente. Isso significa que cada componente e seus filhos terão sua própria instância do serviço.

### Exemplo de Gestão de Dependência

Aqui está um exemplo simples de como gerenciar dependências em um aplicativo Angular:

```typescript
// my-service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class MyService {
  constructor() { }
  getData() {
    return 'Some data';
  }
}

// my-component.ts
import { Component, OnInit } from '@angular/core';
import { MyService } from './my-service';

@Component({
  selector: 'app-my-component',
  template: `<div>{{ data }}</div>`,
  providers: [MyService] // Local provider
})
export class MyComponent implements OnInit {
  data: string;
  
  constructor(private myService: MyService) { }
  
  ngOnInit() {
    this.data = this.myService.getData();
  }
}
```

Neste exemplo, `MyService` é injetado em `MyComponent`. Se `MyService` fosse provido no `root`, haveria uma única instância para todo o aplicativo. Como ele é provido no `providers` do componente, cada instância de `MyComponent` teria sua própria instância de `MyService`.

### Conclusão

A gestão de dependência no Angular é feita através de provedores declarados em diferentes níveis (root, módulo, componente), controlando assim o tempo de vida dos serviços. Ao entender essa hierarquia e os diferentes escopos, você pode gerenciar dependências de forma eficiente e criar aplicativos modulares e testáveis.
