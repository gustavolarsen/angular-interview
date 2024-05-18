# Como funciona o OnPush e como ele ajuda a melhorar a performance da aplicação?

Por padrão, o Angular usa a estratégia de detecção de mudanças `default`, onde toda a árvore de componentes é verificada cada vez que ocorre uma mudança no estado da aplicação. Isso pode ser custoso em termos de desempenho, especialmente em aplicações grandes e complexas.

A estratégia de detecção de mudanças OnPush é uma opção para otimizar a performance de aplicações. Ao reduzir o número de verificações de mudanças desnecessárias, ela permite que o Angular se concentre apenas nas mudanças relevantes, tornando as aplicações mais eficientes e responsivas.

### Estratégia de Detecção de Mudanças OnPush

A estratégia de detecção de mudanças OnPush é uma alternativa que pode melhorar significativamente a performance. Quando um componente é configurado com a estratégia OnPush, o Angular só verifica esse componente para mudanças em situações específicas, em vez de toda vez que ocorre uma mudança em qualquer lugar da aplicação. Essas situações incluem:

1. **Entrada de Dados (Input) Imutável**: Quando as propriedades @Input de um componente recebem novos valores (que são diferentes das referências anteriores).
2. **Eventos do Componente**: Quando um evento de usuário ocorre no componente, como cliques ou entradas de teclado.
3. **Observáveis e Promises**: Quando um observável ou promise associado ao componente emite um novo valor ou é resolvido.

### Como Configurar OnPush

Para configurar um componente com a estratégia OnPush, você define a estratégia de detecção de mudanças no decorador @Component, na prop `changeDetection`

```typescript
import { Component, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-meu-componente',
  templateUrl: './meu-componente.component.html',
  styleUrls: ['./meu-componente.component.css'],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class MeuComponenteComponent {
  @Input() dados: any;
}
```

### Benefícios de Usar OnPush

1. **Menos Verificações**: Como a detecção de mudanças é limitada a mudanças de entrada, eventos e assinaturas de observáveis, o Angular realiza menos verificações de mudanças, economizando recursos.
2. **Melhoria de Performance**: Em aplicações grandes, essa estratégia pode resultar em melhorias significativas de desempenho, especialmente quando a maioria dos componentes não precisa ser verificada constantemente.
3. **Previsibilidade**: OnPush ajuda a tornar o comportamento do aplicativo mais previsível, pois as verificações de mudanças são desencadeadas apenas por condições bem definidas.

### Exemplo Prático

Vamos supor que temos um componente de lista de usuários onde a lista é atualizada frequentemente. Usando OnPush, podemos garantir que o componente de usuário individual só seja atualizado quando seu @Input (o próprio usuário) mudar, não quando a lista inteira muda:

```typescript
import { Component, Input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-usuario',
  template: `
    <div>
      {{ usuario.nome }}
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UsuarioComponent {
  @Input() usuario: { nome: string };
}

@Component({
  selector: 'app-lista-usuarios',
  template: `
    <app-usuario *ngFor="let usuario de usuarios" [usuario]="usuario"></app-usuario>
  `
})
export class ListaUsuariosComponent {
  usuarios = [{ nome: 'Alice' }, { nome: 'Bob' }];
}
```
