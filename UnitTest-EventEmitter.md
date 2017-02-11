### How to make a unit test for `Ouput property` using `EventEmitter`

In this post, I will explain how to do unit test for Output property using EventEmitter in Angular 2.

While we make unit tests for components or directives we may need to simulate `mouse` or `keyboard` events. `dispatchEvent` will be used for this purpose. 

```
...
// create KeyboardEvent object
beforeEach(() => {
  inputEl = el.querySelector('input');
  keydownEvent = new KeyboardEvent('keydown', {bubbles: true, cancelable: true, shiftKey: true});
  keyupEvent = new KeyboardEvent('keyup', {bubbles: true, cancelable: true, shiftKey: true});
});
...

// you can add other event properties using Object.defineProperty
it ('should not show select items until less than minLength', () => {
  spyOn(cmp, 'eventHandler').and.returnValue(true);
  Object.defineProperty(keyupEvent, 'keyCode', {value: 66});
  Object.defineProperty(keyupEvent, 'target', {value: {value: 'ab'}});
  inputEl.dispatchEvent(keyupEvent);

  expect(cmp.eventHandler).not.toHaveBeenCalled();
});

```

However, custom events made with EventEmitter can't be tested with `dispatchEvent` method. When we think that EventEmitter creates an Observable and it triggers a custome event from a child component and propagates to the parent or consumer compoent, we can make the unit test using `subscribe` to the observable.

```
import {Component, Output, EventEmitter} from 'angular2/core';
@Component({
    selector:'my-radio',
    template:`<radio (click)="clickRed()">Red</radio> <radio (click)="clickGreen()">Green</radio>`
})
export class MyRadio {
    @Output() redClicked = new EventEmitter();
    @Output() greenClicked = new EventEmitter();
    
    clickRed() {
      this.redClicked.emit('red');
    }
    
    clickGreen() {
      this.greenClicked.emit('green');
    }
}
@Component({
    selector:'color-radio',
    template:`<MyRadio (redClicked)="onRedCliked($event)" (greenClicked)="onGreenClicked($event)"></MyRadio>`,
    directives:[MyRadio]
})
export class ColorRadio {
    onRedClicked(e) {
        console.log(e);
    }
}

describe('My radio component', () => {
   it ('should emit redClicked event',
      injectAsync([TestComponentBuilder], (tcb: TestComponentBuilder) => {
        return tcb
          .createAsync(ColorRadio)
          .then((componentFixture: ComponentFixture) => {
            fixture = componentFixture;
            cmp = fixture.componentInstance;
            el = fixture.debugElement;
  
            let myRadioCmp = el.query(By.css('MyRadio')).componentInstance;
            
            myRadioCmp.redClicked.subscribe((data: any) => {
              expect(data).toBe('red');
            });
            
            myRadioCmp.redClicked.emit('red');
          }
        );
      })
    );
});

```
