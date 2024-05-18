# Como controlar vazamentos de memória (Memory leak)

Controlar leaks de memória é crucial para manter a performance e a eficiência da aplicação ao longo do tempo. Existem algumas algumas práticas recomendadas para ajudar a controlar e mitigar vazamentos de memória em Angular:

### 1. **Gerenciamento de Assinaturas**
   - **Unsubscribe**: Sempre desinscreva-se de observáveis (`Observables`) quando o componente for destruído. Utilize o operador `takeUntil` ou mantenha uma referência às assinaturas e chame o `unsubscribe` manualmente.
     ```typescript
     import { Subject } from 'rxjs';
     import { takeUntil } from 'rxjs/operators';

     private destroy$ = new Subject<void>();

     ngOnInit() {
       this.myService.getData().pipe(
         takeUntil(this.destroy$)
       ).subscribe(data => {
         // Handle data
       });
     }

     ngOnDestroy() {
       this.destroy$.next();
       this.destroy$.complete();
     }
     ```

### 2. **Evitar Manipulação Direta do DOM**
   - Use os mecanismos do Angular (como `@ViewChild` e `Renderer2`) para manipular o DOM em vez de acessar diretamente elementos DOM com `document.querySelector` ou `element.nativeElement`.

### 3. **Destruir Timers e Intervals**
   - Certifique-se de limpar quaisquer `setTimeout`, `setInterval` ou `requestAnimationFrame` que você criou.
     ```typescript
     private intervalId: any;

     ngOnInit() {
       this.intervalId = setInterval(() => {
         // Your logic here
       }, 1000);
     }

     ngOnDestroy() {
       if (this.intervalId) {
         clearInterval(this.intervalId);
       }
     }
     ```

### 4. **Evitando Detecção de Mudanças Excessiva**
   - Utilize a estratégia de detecção de mudanças `OnPush` quando possível para reduzir a carga de detecção de mudanças.
     ```typescript
     @Component({
       selector: 'app-my-component',
       templateUrl: './my-component.component.html',
       changeDetection: ChangeDetectionStrategy.OnPush
     })
     export class MyComponent { }
     ```

### 5. **Uso Correto de Diretivas e Componentes**
   - Certifique-se de remover corretamente quaisquer diretivas ou componentes dinâmicos adicionados através de `ComponentFactoryResolver`.

### 6. **Destruição de Serviços Singleton**
   - Para serviços que são fornecidos no nível do módulo (`providedIn: 'root'`), certifique-se de que você não está mantendo referências desnecessárias que podem impedir a coleta de lixo.

### 7. **Profiling e Ferramentas de Diagnóstico**
   - Utilize ferramentas como Chrome DevTools, o Angular DevTools e bibliotecas como RxJS DevTools para monitorar e diagnosticar possíveis vazamentos de memória.
   - No Chrome DevTools, você pode tirar snapshots de heap e comparar para identificar objetos que não foram coletados pelo garbage collector.

### 8. **Evitar Variáveis Globais**
   - Evite o uso de variáveis globais que podem reter referências indesejadas na memória.

### 9. **Uso de Async Pipe**
   - Utilize o `async` pipe nos templates para se inscrever e desinscrever automaticamente de observáveis.

     ```html
     <div *ngIf="data$ | async as data">
       {{ data }}
     </div>
     ```

### 10. **Desalocação de Recursos**
   - Certifique-se de desalocar corretamente quaisquer recursos, como objetos grandes, conexões de WebSocket ou recursos nativos que você tenha alocado.
