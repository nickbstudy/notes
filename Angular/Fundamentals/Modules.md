`ng g m module-name-here` to generate.

Modules are a container for components, custom pipes, services, and routes.  They can also use other module, or be exported.  Lazy loading can be used to improve performance ==Not covered yet==

A boilerplate module with a couple of components added will look like `user.module.ts`
```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { SignInComponent } from './sign-in/sign-in.component';
import { TemplateFormControlsComponent } from './template-form-controls/template-form-controls.component';
import { FormsModule } from '@angular/forms';


@NgModule({
  declarations: [
    SignInComponent,
    TemplateFormControlsComponent
  ],
  imports: [
    CommonModule,
    FormsModule
  ]
})
export class UserModule { }
```