## Property Binding vs Attribute Binding

In Angular 2, when we do data binding, it works with properties and events not attributes.(period!) It is very important to understand template binding in Angular 2. In Angular 1, attributes are used for data binding. But in Angular 2, the role of attributes is to initialize element and directive state. Attribute's values are treated as a string.

If we don't know about it we might make mistake like below.

```
@Component({
  selector: 'my-directive'
})
export class MyDirective {
  @Input() attr1: string;
  @Input() attr2: number;
  @Input() attr3: boolean;
  
  ...
}

<my-directive attr1="abc" attr2="1" attr3="true" > </my-directive>
```
attr1, attr2 attr3 are defined as Inputs in the component. The type of the attributes are all string but in the component they are defined as `string`, `number` and `boolean`. attr1 and attr2 might be ok. But attr3 will not work properly unless it is compared like attr3 === "true" but it was not intention. 

If they are readonly values and not changed during run time, it can be used with attribute binding. But if you want to change it during run time, and evaluate properly as a boolean then you should use property binding like below

```
<my-directive [attr1]="'abc'" [attr2]="1" [attr3]="true" > </my-directive>
```

Please read the reference from the Angular official site.

### Reference
- [HTML Attribute vs DOM Property](https://angular.io/docs/ts/latest/guide/template-syntax.html#!#binding-syntax)
