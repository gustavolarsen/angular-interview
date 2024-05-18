# Explique o lazy loading e por quê deve ser utilizado?

O **Lazy Loading** (carregamento sob demanda) é uma técnica que permite carregar módulos ou componentes em uma aplicação Angular apenas quando eles são realmente necessários, em vez de carregar todos os recursos de uma vez no início. Isso pode melhorar significativamente o desempenho inicial da aplicação, especialmente em aplicações grandes.

### Como Funciona o Lazy Loading no Angular

No Angular, o lazy loading é implementado principalmente através do carregamento sob demanda de módulos de funcionalidade, conhecidos como **feature modules**. Em vez de incluir esses módulos diretamente na configuração de roteamento principal, eles são carregados dinamicamente quando o usuário navega para uma rota específica que requer o módulo.

### Configuração de Lazy Loading

Para configurar o lazy loading, você deve organizar a aplicação em módulos e definir rotas que utilizam a propriedade `loadChildren` para carregar esses módulos sob demanda. 

### Estrutura de Módulos

Suponha que temos uma aplicação com um módulo principal (`AppModule`) e um módulo de funcionalidade (`FeatureModule`).

1. **FeatureModule**:
    ```typescript
    import { NgModule } from '@angular/core';
    import { CommonModule } from '@angular/common';
    import { RouterModule, Routes } from '@angular/router';
    import { FeatureComponent } from './feature.component';

    const routes: Routes = [
      { path: '', component: FeatureComponent }
    ];

    @NgModule({
      declarations: [FeatureComponent],
      imports: [
        CommonModule,
        RouterModule.forChild(routes)
      ]
    })
    export class FeatureModule { }
    ```

2. **AppModule**:
    ```typescript
    import { NgModule } from '@angular/core';
    import { BrowserModule } from '@angular/platform-browser';
    import { RouterModule, Routes } from '@angular/router';
    import { AppComponent } from './app.component';
    import { HomeComponent } from './home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'feature', loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule) }
    ];

    @NgModule({
      declarations: [
        AppComponent,
        HomeComponent
      ],
      imports: [
        BrowserModule,
        RouterModule.forRoot(routes)
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

### Como Funciona

- **Rotas**: No `AppModule`, a rota `'feature'` está configurada para carregar o `FeatureModule` sob demanda. Quando o usuário navega para `/feature`, o Angular carrega o `FeatureModule` dinamicamente.
- **loadChildren**: A função `loadChildren` utiliza a sintaxe de importação dinâmica do JavaScript para carregar o módulo. Isso resulta em uma divisão de código, onde o módulo é carregado em um pacote separado (chunk).

### Benefícios do Lazy Loading

1. **Performance Inicial Melhorada**: Reduz o tamanho inicial do pacote JavaScript que é carregado quando a aplicação é iniciada, diminuindo o tempo de carregamento inicial.
2. **Carregamento Sob Demanda**: Módulos são carregados apenas quando necessário, economizando recursos e largura de banda.
3. **Escalabilidade**: Facilita o gerenciamento de grandes aplicações, permitindo uma estrutura modular mais clara e organizada.
