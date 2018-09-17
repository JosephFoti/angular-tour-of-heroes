## Angular notes

#### CLI

  - new project | ng new [project name]
  - run server* | ng serve
  - generate elements
    - _Generated elements come with bundles of component files and automatically link into the app module at the top level of ng_
    - generate new component | ng generate component [component name]
    - generate new service | ng generate service [service]
    - generate new router | ng generate module app-routing --flat --module=app
      - flat puts the file in src/app instead of its own folder.
      - module=app tells the CLI to register it in the imports array of the AppModule.

  * in project directory

#### One-way binding

  - Handling parent child relationships
    - render child in template by using its selector property and incorporating it as a tag
    ```html
      <my-kiddo></my-kiddo>
    ```
    - pass property by importing and implementing Input package to the child component. First import ...

    ```typescript
    import { Component, OnInit, Input } from '@angular/core';
    //                          ^^^^^
    ```

    - then add the import tag to the class. the first param is the name of the var to be passed in, followed by the custom type Hero

    ```typescript
    export class example {
      @Input() hero: Hero;
      //...
    }
    ```

    - pass the var in the parent template with a value

    ```html
    <app-example [var]="[value, can be var]"></app-example>
    ```

#### Two-way binding

  - Configuring ng to process and change data from the client, and render data from the code.
  - must add FormsModule to app.module.ts, main controller for app.

  ```typescript

  import { FormsModule } from '@angular/forms';

  // and add FormsModule to 'imports' in @NgModule

  @NgModule({
    declarations: [
      AppComponent,
      HeroesComponent
    ],
    imports: [
      BrowserModule,
      FormsModule // <-- here
    ],
    providers: [],
    bootstrap: [AppComponent]
  });

  ```

  ```html
  <!-- template -->
  <div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name">
    </label>
  </div>

  ```

#### ng Template Methods

  - \*ngFor
    - Loops through defined iterator from component module
    - syntax | \*ngFor="[for loop statement]"
      - e.g.
      ```html
      <element *ngFor="let item of array">
        <h1>{{item.prop}}</h1>
        <h3>{{item.prop2}}</h3>
      </element>
      ```

  - \*ngIf
    - Conditional working with component module
    - syntax | \*ngIf="[conditional statement]"
      - e.g.
      ```html
      <element *ngIf="variable === true">
        <h1>Show me if condition is true</h1>
      </element>
      ```

  - Class Binging
    - Conditional class binder for a given class if a condition is met
    - syntax | [class.[classname]]="[conditional statement]"
      - e.g.
      ```html
      <element [class.selected]="clicked === true">
        <h1>Parent receives class if condition is met</h1>
      </element>
      ```

#### Services

  - Used to provide and handle external data. Bound to constructor props.
    - syntax
    ```typescript
    import { Injectable } from '@angular/core';
    @Injectable({
      providedIn: 'root'
    })
    export class ExampleService {

      getData(): [type] {
        // Process data
        return data;
      }

    }

    // ---- recieving in component ----

    // imports ...
    import { ExampleService } from '../example.service';

    // class export ...

    constructor([private || public] exampleService: ExampleService) { }
    //          ^ private for vars scoped to constructor, public for vars scoped for linked template

    getData(): void {
      this.var = this.exampleService.getData();
    }

    ngOnInit() {
      this.getData()
    }
    ```
    - Above process describes a synchronus call. To make async use Observable

#### Observables

  - Additional library for handling asynchronus calls and promises. [docs](https://rxjs-dev.firebaseapp.com/).
  - syntax
  ```typescript
  import { Injectable } from '@angular/core';
  import { Observable, of } from 'rxjs';

  @Injectable({
    providedIn: 'root'
  })
  export class ExampleService {

    getData(): Observable<type> {
      // Process data
      return of(data);
    }

  }

  // ---- recieving in component ----

  // imports ...
  import { ExampleService } from '../example.service';

  // class export ...

  constructor([private || public] exampleService: ExampleService) { }
  //          ^ private for vars scoped to constructor, public for vars scoped for linked template


  getData(): void {
    this.exampleService.getData()
      .subscribe(data => this.var = data);
  }

  ngOnInit() {
    this.getData();
  }
  ```
