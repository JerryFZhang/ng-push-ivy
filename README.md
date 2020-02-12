# ng-push-ivy

Angular Push Notifications API Wrapper

[![NPM Version](https://img.shields.io/npm/v/ng-push-ivy.svg)](https://www.npmjs.com/package/ng-push-ivy)
[![NPM Downloads](https://img.shields.io/npm/dt/ng-push-ivy.svg)](https://www.npmjs.com/package/ng-push-ivy)

If you aren't familiar with push notifications you can read more about them on the [Mozilla developer network](https://developer.mozilla.org/en-US/docs/Web/API/Notification).

## Installation

To install this library, run:

```bash
$ npm install ng-push-ivy --save
```

## Setup

Import the `PushNotificationsModule` in to your `AppModule`:
```ts
@NgModule({
    imports: [PushNotificationsModule],
    declarations: [AppComponent],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

Now import the `PushNotificationsService` where you want to use it: 

```ts
constructor( private _pushNotifications: PushNotificationsService ) {}
```

## Requesting Permission

To request permission from the user to display push notifications call the `requestPermission()` method on the `PushNotificationsService`.

## Create Notification

You can create a notification calling the `create(title: string, options: PushNotification)` method, like this: 

```ts
this._pushNotifications.create('Test', {body: 'something'}).subscribe(
            res => console.log(res),
            err => console.log(err)
        )
```

The `create()` method returns an observable that emits the notification and the event when ever a show, click, close or error event occurs.

If you don't have permission to display the notification then the `Permission not granted` error will be thrown.

## Options

The following are options that can be passed to the options parameter: 

```ts
interface PushNotification {
    body?: string
    icon?: string
    tag?: string
    renotify?: boolean
    silent?: boolean
    sound?: string
    noscreen?: boolean
    sticky?: boolean
    dir?: 'auto' | 'ltr' | 'rtl'
    lang?: string
    vibrate?: number[]
}
```

Options correspond to the Notification interface of the Notification API:
[Mozilla developer network](https://developer.mozilla.org/en-US/docs/Web/API/Notification).

## Examples

### Registering to click event

```ts
this._pushNotifications.create(
    'Example One',
    {body: 'Just an example'}
)
    .subscribe(res => {
        if (res.event.type === 'click') {
            // You can do anything else here
            res.notification.close();
        }
    }
)
```

### Using with universal method one (using injector)

Thank you [@anode7](https://github.com/anode7) for submitting this example

```ts
import {Component, Inject, PLATFORM_ID, Injector} from '@angular/core';
import {isPlatformBrowser} from '@angular/common';

@Component({})
export class ExampleComponent {
    private _push:PushNotificationsService;

    constructor(
        @Inject(PLATFORM_ID) platformId: string,
        private injector: Injector,
    ) {
        if (isPlatformBrowser(platformId)) {
            //inject service only on browser platform
            this._push = this.injector.get(PushNotificationsService);
        }
    }
}
```

### Using with universal method two (mock service)

A standard method used in Universal is creating a mock service which is injected in the `ServerModule`. A good example can be found [here](https://github.com/angular/universal-starter/issues/148).

## Development

```bash
$ npm run build
```

## License

MIT © [JerryFZhang](mailto:hi@JerryFZhang)
