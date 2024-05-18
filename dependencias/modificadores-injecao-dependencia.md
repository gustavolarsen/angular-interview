# Quais são os modificadores de injeção de dependência?

No Angular, a injeção de dependência (Dependency Injection, DI) é uma técnica que permite que as dependências de uma classe sejam fornecidas em vez de serem criadas pela própria classe. Isso promove um código mais modular e testável. Existem vários modificadores que você pode usar para controlar como e onde as dependências são injetadas.

1. **@Injectable()**: Este decorador marca uma classe como disponível para ser fornecida e injetada como uma dependência. Você pode especificar o provedor da classe dentro deste decorador.
   ```typescript
   @Injectable({
     providedIn: 'root',
   })
   export class MyService { }
   ```

2. **@Inject()**: Usado para indicar explicitamente um token a ser injetado. Isso é útil quando a classe ou o token tem um nome que pode ser ofuscado, ou para injetar algo diferente de uma classe.
   ```typescript
   constructor(@Inject('MyToken') private myToken) { }
   ```

3. **@Optional()**: Marca uma dependência como opcional. Se o Angular não puder encontrar um provedor para a dependência, ele injeta `null` em vez de lançar um erro.
   ```typescript
   constructor(@Optional() private myOptionalService: MyService) { }
   ```

4. **@Self()**: Restringe a resolução da dependência ao injetor atual. Se a dependência não puder ser encontrada no injetor atual, um erro será lançado.
   ```typescript
   constructor(@Self() private myService: MyService) { }
   ```

5. **@SkipSelf()**: Ignora o injetor atual e procura a dependência em injetores pais.
   ```typescript
   constructor(@SkipSelf() private parentService: MyService) { }
   ```

6. **@Host()**: Restringe a resolução da dependência ao componente hospedeiro, ignorando injetores filhos.
   ```typescript
   constructor(@Host() private hostService: MyService) { }
   ```

### Exemplos de Uso

#### @Injectable
```typescript
@Injectable({
  providedIn: 'root',  // O serviço é um singleton e é disponibilizado em toda a aplicação.
})
export class MyService {
  // implementação do serviço
}
```

#### @Inject
```typescript
@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
})
export class MyComponent {
  constructor(@Inject('MyToken') private myToken) { 
    console.log(myToken);
  }
}
```

#### @Optional
```typescript
@Component({
  selector: 'app-optional-service',
  templateUrl: './optional-service.component.html',
})
export class OptionalServiceComponent {
  constructor(@Optional() private myOptionalService: MyService) {
    if (myOptionalService) {
      // Serviço disponível
    } else {
      // Serviço não disponível
    }
  }
}
```

#### @Self
```typescript
@Component({
  selector: 'app-self-service',
  templateUrl: './self-service.component.html',
})
export class SelfServiceComponent {
  constructor(@Self() private myService: MyService) {
    // Usado somente se o serviço estiver disponível no injetor atual
  }
}
```

#### @SkipSelf
```typescript
@Component({
  selector: 'app-skip-self-service',
  templateUrl: './skip-self-service.component.html',
})
export class SkipSelfServiceComponent {
  constructor(@SkipSelf() private parentService: MyService) {
    // Ignora o injetor atual e procura nos injetores pais
  }
}
```

#### @Host
```typescript
@Component({
  selector: 'app-host-service',
  templateUrl: './host-service.component.html',
})
export class HostServiceComponent {
  constructor(@Host() private hostService: MyService) {
    // Restringe a resolução ao componente hospedeiro
  }
}
```

Com esses modificadores, você tem controle refinado sobre como e onde as dependências são resolvidas e injetadas, permitindo flexibilidade e robustez no design de sua aplicação Angular.
