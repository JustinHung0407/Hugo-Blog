---
title: "Angular Learning"
date: 2019-12-03T00:44:14+08:00
draft: false
toc: true
tags:
  - Programming Languages Learning
---

Learning from : [Angular 深入淺出三十天](https://ithelp.ithome.com.tw/users/20090728/ironman/1600)

## Startup
- node -v
- npm -v
- npm install -g @angular/cli
- ng -v

- ### ng-cli
    - `ng generate <schematic> [options]`
    - `ng generate application`
    - `ng generate library`

        * class
        * component
        * directive
        * enum
        * guard
        * interface
        * module
        * pipe
        * service

- ### Hello World
    - ng new HelloAngular
    - cd HelloAngular
    - ng serve
    - localhost:4200

- ### Tools
    - VS code
    - Chrome Extention : Augury

## Directory Structure
1. .editorconfig - 不同的編輯器和 IDE 之間更容易定義與維護一致的編碼樣式。
2. angular.json - Angular CLI 的設定檔
    (`ng run <project>:<architect>[:configuration] [...options]`)
    ``` json
        {
          "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
          (用來放置 angular.json 可以使用的相關設定說明檔案位置)
          "version": 1,
          "newProjectRoot": "projects",
          "projects": {
          (放置所有可以被管理的專案, monorepo)
            "demo": {
              "root": "",
              "sourceRoot": "src",
              "projectType": "application",
              (application or library)
              "prefix": "app",
              (CSS selector prefix)
              "schematics": {},
              (@angular-devkit/schematics套件之相關設定)
              "architect": {
              (可以使用的指令名稱)
                 "serve": {
                    "builder": "@angular-devkit/build-angular:dev-server",
                    "options": {
                       "browserTarget": "demo:build"
                    },
                    "configurations": {
                    (不同的模式要替換的參數)
                      "production": {
                        "browserTarget": "demo:build:production"
                       }
                     }
                  }    
              }
            },
            ...
          },
          "defaultProject": "demo"
        }
    ```

5. dist/ - 編譯且打包過後的程式碼會放在這裡。一開始其實並不會看到此資料夾，是我為了介紹結構的關係，才先下指令讓其出現。

6. e2e/ - E2E 測試 （End-to-End Testing） 的程式碼要擺放的位置。

7. node_modules/ - npm install 安裝後，都會擺放到這裡。

8. package-lock.json - npm install 安裝套件時所產生的文件，用以記錄當前實際安裝的套件的來源與版號。

9. package.json - 這個檔案是用來定義像是應用程式的名稱、版本、描述、關鍵字、授權、貢獻者、維護者、腳本、相依的相關套件及其版本資訊等等，詳細請參考官方文件的說明。
10. src/ - 主要在開發的所有程式碼都會放在這裡。
11. tsconfig.json - 這個檔案是 TypeScript 編譯時看的編譯設定檔。
12. tslint.json - TSLint 是 TypeScript 的格式驗證工具，它可以檢查你寫出來的 TypeScript 的格式是不是具有可讀性、可維護性和功能性錯誤。



## Angular Main components
![](https://angular.tw/generated/images/guide/architecture/overview2.png)

  - ## NgModule
    - 一個檔案大致上切分為三個區塊
        - import
        - Decorator - 傳入裝飾器裡的物件=metadata
            - declarations－屬於此 NgModule 的 Component、Directive 與 Pipe 皆放置於此。
            - imports－此 NgModule 需要使用、依賴的其他 NgModule 皆放置於此
            - providers－可以被整個應用程式中的任何部分被使用的 Service 皆放置於此，也可以將 Service 直接放置在 Component 的 Metadata 裡的 providers （主要是用來決定哪些服務(service)允許被注入） 。
                - 基本架構
                    - provide: SomeClass ：代表要提供的注入內容是什麼，這時後我們會把設定的類別當作是一個 token
                    - useXXXX: YYYY：useXXXX 代表多種設定，包含 useClass、 useExisting、useValue和useFactory
                    ```json=
                    {
                      provide: SomeClass,
                      useXXXX: YYYY
                    }
                    ```
                - useClass - 使用某個類別，來當做產生 token 的實體
                    ```json=
                    {
                      provide: SomeClass,
                      useClass: AnotherClass
                    }
                    ```
                - useExisting - 使用 useClass 則一定會產生新的實體，因此我們會改成使用 useExisting 來指定已經產生過實體當作 token
                    ```typescript=
                    @NgModule({
                          providers: [
                            // 原來的 SystemLogger
                            SystemLogger,
                            {
                              // 直接使用 SystemLogger 產生過的實體，而不重新建立
                              provide: GlobalLogger,
                              useExisting: SystemLogger
                            }
                          ]
                    })
                    ```
                - useValue - 若是單純的建立一個跟原來的 service 一樣簽章的物件，可以使用 useValue 來替換掉原來的類別
                ```typescript=
                    @NgModule({
                      providers: [
                        {
                          provide: DataService,
                          useValue: newData,
                        }
                      ]
                    })
                ```
                - useFactory - 以工廠方法來幫忙生產實體，避免建立實體時會有更多的複雜邏輯
                ```typescript=
                const dataServiceFactory = () => {
                  return new AnotherDataService();
                }

                @NgModule({
                  providers: [
                    {
                      provide: DataService,
                      useFactory: dataServiceFactory
                    }
                  ]
                })
                ```
            - exports－此處放置的是，當在其他 NgModule 裡 import 了當前的 Module 後，可以在其他 NgModule 裡的 Component Template 使用的 Component、Directive 與 Pipe。
            - entryComponents－放在這裡的元件通常是用不通過 Route 的方式，而採用動態加入的元件。
            - bootstrap－bootstrap : [] 裡面的元件會自動被啟動，在此設置的是應用程式通常稱之為 Root Component （根元件） ，而且只有 Root Module 才要設置此屬性。
        - Class
            - `export class AppModule { }`
        - 模組切割概念(in common) [模組化的基本觀念](https://wellwind.idv.tw/blog/2018/10/21/mastering-angular-06-basic-modularize/)
            - FeatureModule
                - UserModule, DropdownModule
            - SharedModule
                - module復用，也可以互相引用達到共享設定的目的
            - CoreModule
                - 避免每個模組都重新產生一次service(若只在 AppModule 加入 CoreModule 其他模組內找不到 service 時就會向外找到 AppModule 中 CoreModule 內的 service 此時就可以確保只拿到一個實體)

                ```typescript=
                @Injectable()
                export class SearchService { }

                @NgModule({
                  providers: [SearchService]
                })
                export class CoreModule { }

                @NgModule({
                  // 若在此加入 CoreModule，將會取得不同的實體
                })
                export class MainModule { }
                @NgModule({
                  imports: [BrowserModule, CoreModule, MainModule],
                  ...
                })
                export class AppModule { }
                ```



  - ## Component
    - Life Cycle
        [完整的元件生命週期介紹](https://www.facebook.com/will.fans/videos/1875672075795261/?v=1875672075795261)
        * construture
        * ngOnChanges()	
        * ngOnInit()	
        * ngDoCheck()	
        * ngAfterContentInit()	
        * ngAfterContentChecked()	
        * ngAfterViewInit()	
        * ngAfterViewChecked()	
        * ngOnDestroy()
    - 負責定義與控制畫面, 令其可以根據資料和程式邏輯呈現相對應的畫面
    - 程式分三部分
        -  import 
        -  @Component 
            -  selector－CSS 選擇器，它告訴 Angular 在 Template 中找到相應的位置之後，創建並插入該Component 的實體。Angular 會在 Template 中尋找 `<app-root></app-root>` 標籤，創建出 AppComponent 的實體並將其插入其中。
            -  templateUrl－此 Component 的 Template 檔案位置 （相對位置)
            -  styleUrls－ Component 的樣式檔
            -  providers－跟 NgModule 的 providers 類似
        -  Lifecycle hooks - 可以在程式中加入對應生命週期的方法，來得知生命週期變化，以及作出對應的處理
        - e.g. ngOnInit(),  ngOnChanges(), AfterContentInit,  AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy
            ```typescript=
            export class ArticleBodyComponent implements OnInit, OnChanges{
              @Input()
              item;

              @Input()
              counter;

              constructor() {
                console.log('ArticleBodyComponent: constructor');
              }

              ngOnInit() {
                console.log(`ArticleBodyComponent ${this.item.id} : ngOnInit`);
              }

              ngOnChanges() {
                console.log(`ArticleBodyComponent ${this.item.id} : ngOnChanges`);
              }
            }
            ```
            ```htmlmixed=
            <article class="post" id="post{{idx}}" *ngFor="let item of atticleData; let idx = index">
              <app-article-header [item]="item" (delete)="doDelete($event)" (changeTitle)="doChange($event)"></app-article-header>
              <app-article-body [item]="item" [counter]="counter"></app-article-body>
            </article>
            ```


  - ## Router
    - `ng generate module app-routing --flat --module=app`
    - `--flat` 為輸出文件至 src/app
    - `--module=app` 註冊此Routing至AppModule的imports
    - Passing params : 
        - e.g. detail/:id
        - write in Component
        ```typescript=
            ngOnInit(): void {
              this.getHero();
            }

            getHero(): void {
              const id = +this.route.snapshot.paramMap.get('id');//用route.snapshot.paramMap.get取得Routing時傳入的變數
              this.heroService.getHero(id)
                .subscribe(hero => this.hero = hero);
            }
        ```
    - route
    ```typescript=
    const appRoutes: Routes = [
      { path: 'crisis-center', component: CrisisListComponent },
      { path: 'heroes', component: HeroListComponent },
    ];
    ```

    - Lazy Load
        預先載入跟延遲載入很像，差別只在於，
        - 延遲模組是要進入該路由的時候即時載入；
        ex. don't import from '**app.module.ts**'
        ```jsonld=
        {
          path: 'feature',
          loadChildren: './feature/feature.module#FeatureModule'
        }
        ```
        - 預先載入是在頁面初始化的時候就把所有可延遲載入的模組透過背景非同步地下載，不會影響畫面的顯示或使用者的操作。
            - ex. in the route module
            ```angular
            import { PreloadAllModules } from '@angular/router';
            ```
            - then in the same file in (RouterModule.forRoot)
                add **preloadingStrategy: PreloadAllModules**
            ```angular
            @NgModule({
              imports: [RouterModule.forRoot(routes, {
                useHash: true,
                preloadingStrategy: PreloadAllModules
              })],
              exports: [RouterModule],
              providers: []
            })
            ```


 - ## Directive
    - 結構型
        - NgIf
        ```htmlmixed=
        <div [ngSwitch]="hero?.emotion">
          <app-happy-hero    *ngSwitchCase="'happy'"    [hero]="hero"></app-happy-hero>
          <app-sad-hero      *ngSwitchCase="'sad'"      [hero]="hero"></app-sad-hero>
          <app-confused-hero *ngSwitchCase="'confused'" [hero]="hero"></app-confused-hero>
          <app-unknown-hero  *ngSwitchDefault           [hero]="hero"></app-unknown-hero>
        </div>
        ```
        - NgFor
        ```htmlmixed=
        <div *ngFor="let hero of heroes; let i=index; let odd=odd; trackBy: trackById" [class.odd]="odd">
          ({{i}}) {{hero.name}}
        </div>

        <ng-template ngFor let-hero [ngForOf]="heroes" let-i="index" let-odd="odd" [ngForTrackBy]="trackById">
          <div [class.odd]="odd">({{i}}) {{hero.name}}</div>
        </ng-template>
        ```
        - NgSwitch

        ```htmlembedded=
        <div *ngIf="hero" class="name">{{hero.name}}</div>

        <ul>
          <li *ngFor="let hero of heroes">{{hero.name}}</li>
        </ul>

        <div [ngSwitch]="hero?.emotion">
          <app-happy-hero    *ngSwitchCase="'happy'"    [hero]="hero"></app-happy-hero>
          <app-sad-hero      *ngSwitchCase="'sad'"      [hero]="hero"></app-sad-hero>
          <app-confused-hero *ngSwitchCase="'confused'" [hero]="hero"></app-confused-hero>
          <app-unknown-hero  *ngSwitchDefault           [hero]="hero"></app-unknown-hero>
        </div>
        ```
        - 寫一個名叫 UnlessDirective 的結構型指令
        ```typescript=
        import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';
        /**
         * Add the template content to the DOM unless the condition is true.
         */
        @Directive({ selector: '[appUnless]'})
        export class UnlessDirective {
          private hasView = false;

          constructor(
            private templateRef: TemplateRef<any>,
            private viewContainer: ViewContainerRef) { }

          @Input() set appUnless(condition: boolean) {
            if (!condition && !this.hasView) {
              this.viewContainer.createEmbeddedView(this.templateRef);
              this.hasView = true;
            } else if (condition && this.hasView) {
              this.viewContainer.clear();
              this.hasView = false;
            }
          }
        }
        ```
    - 屬性型
        ```typescript=
        import { Directive, ElementRef, HostListener } from '@angular/core';

        @Directive({
          selector: '[appHighlight]'
        })
        export class HighlightDirective {
          constructor(el: ElementRef) {
             el.nativeElement.style.backgroundColor = 'yellow';
          }

          @HostListener('mouseenter') onMouseEnter() {
              this.highlight('yellow');
          }

          @HostListener('mouseleave') onMouseLeave() {
              this.highlight(null);
          }

          private highlight(color: string) {
              this.el.nativeElement.style.backgroundColor = color;
          }
        }
        ```
        - ` <p appHighlight>Highlight me!</p>`


## LayoutGuard
- 進階應用 : [[Angular]Router Guards](https://blog.kevinyang.net/2017/01/14/angular-router-canactivate/)
- CanActivate : 避免瀏覽到該網址
- CanActivateChild : 讓子路由套用CanActivate規則，避免瀏覽到該網址
- CanDeactivate : 避免離開目前的網址
- Resolve : 在前往瀏覽網頁前先預載資料
- CanLoad : 避免載入非同步的路由設定



## Service
## 注入 token 實體的方法
- 建構式注入 - 在建構式中宣告要注入的 token 實體
    - 靜態
    ```typescript=
    constructor(private dataService: DataService) { }
    ```
    - 動態
    ```typescript=
    import { Injector } from '@angular/core';

    constructor(private injector: Injector) {
      const service = this.injector.get(DataService);
      console.log(service.getData());
    }
    ```


- ## Template syntax
    - for loop : `*ngFor`
    - data binding : `{{ }}`
    - pipe : `date: 'yyyy-MM-dd HH:mm:ss'`


- ## Data Binding
    ```javascript==
    <!-- 雙向綁定 -->
    <input [(ngModel)]="property">

    <!-- 屬性綁定 -->
    <input [ngModel]="property">

    <!-- 事件綁定 -->
    <input (ngModelChange)="handler($event)">

    ```

## AsyncPipe
- 為了解決大量使用RxJS產生的非同步程式
    ```typescript=
    import { Observable } from 'rxjs';

    export class AppComponent {
      todos$: Observable<any[]>;

      constructor(private httpClient: HttpClient) { }

      ngOnInit() {
        this.todos$ = this.httpClient.get<any[]>('https://jsonplaceholder.typicode.com/todos/');
      }
    }
    ```
```htmlmixed=
<li *ngFor="let todo of todos$ | async">{{ todo.title }}</li>
```


## 瀏覽器支援
- 最低可支援到 IE9 瀏覽器

| 瀏覽器     | 支援的瀏覽器版本         |
|-----------|-------------------|
| Chrome    | 最新版             |
| Firefox   | 最新版             |
| Edge      | 最近的兩個主版本    |
| IE        | 11109             |
| IE Mobile | 11                |
| Safari    | 最近的兩個主版本    |
| iOS       | 最近的兩個主版本    |
| Android   | Nougat (7.0) Marshmallow (6.0) Lollipop (5.0, 5.1) KitKat (4.4)|

- ### 解決方式
    - [Polyfills](https://blog.miniasp.com/post/2019/02/03/Angular-CLI-73-Use-ES2015-nomodule-load-polyfills)
    - e.g. need to use web animation
        - cli commend `npm install --save web-animations-js`
        - open polyfills.ts then un-comment `import 'web-animations-js';`



## Angular 7 & 8 新功能
[Angular 7 全新功能探索 - Will](https://www.slideshare.net/WillHuangTW/explore-angular-7-new-features)
- cli - ng new, ng add 互動式介面(route, css select, material)
- Bundle Budget - angular.json =>
- Component Dev Kit (CDK) - improve accessibility UX
- Ivy - 
- Angular Universal - 解決spa難以實踐seo的問題