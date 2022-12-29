# RxJS operators - zip / combineLast / withLatestFrom / forkJoin

Per unire informazioni da più observables bisogna utilizzare i `combination operators`

Immaginiamo di dover stampare una maglietta: abbiamo due signori, mr Color che definisce il colore e mr Logo che definisce il logo. Entrambi scelgono spontaneamente e noi dobbiamo aspettare di avere entrambe le informazioni per stampare la maglietta. Questo esempio rappresenta due observables `color$` e `logo$`

![Untitled](RxJS%20operators%20-%20zip%20combineLast%20withLatestFrom%20fo%20dafafffbeb5145f4962989289589abfd/Untitled.png)

```jsx
// 0. Import Rxjs operators
import { forkJoin, zip, combineLatest, Subject } from 'rxjs';
import { withLatestFrom, take, first } from 'rxjs/operators';

// 1. Define shirt color and logo options
type Color = 'white' | 'green' | 'red' | 'blue';
type Logo = 'fish' | 'dog' | 'bird' | 'cow';

// 2. Create the two persons - color and logo observables,
// They will communicate with us later (when we subscribe)
const color$ = new Subject<Color>();
const logo$ = new Subject<Logo>();

// 3. We are ready to start printing shirts. You need to subscribe to color and logo observables to produce shirts. We will write that code here later.
...

// 4. The two persons(observables) are doing their job, picking color and logo
color$.next('white');
logo$.next('fish');

color$.next('green');
logo$.next('dog');

color$.next('red');
logo$.next('bird');

color$.next('blue');

// 5. When the two persons (observables) have no more info, they say bye. We will write code here later.
...
```

Creiamo due observable usando i Subject. Nella parte 4 del codice ogni next indica una scelta di mr color e mr logo.

![https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/6C1OMCYRQwm2puCgTyXf_sequence.gif](https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/6C1OMCYRQwm2puCgTyXf_sequence.gif)

## zip

```jsx
// 3. We are ready to start printing shirt...
zip(color$, logo$)
    .subscribe(([color, logo]) => console.log(`${color} shirt with ${logo}`));
```

![https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/SYFLX4RmeFoUwjU2lxdQ_zip.gif](https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/SYFLX4RmeFoUwjU2lxdQ_zip.gif)

In questo caso, `color` aspetterà `logo` finchè non arrivano nuovi valori. Entrambi i valori devono cambiare, solo allora il log viene triggerato.

## combineLatest

```jsx
// 3. We are ready to start printing shirt...
combineLatest(color$, logo$)
    .subscribe(([color, logo]) => console.log(`${color} shirt with ${logo}`));
```

![https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/e5wyQseJR9CoQaOi0Kfw_combinelatest.gif](https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/e5wyQseJR9CoQaOi0Kfw_combinelatest.gif)

Qui la prima funzione viene triggerata quando entrambi `color` e `logo` cambiando, da lì ad ogni cambiamento viene triggerato il log.

## withLatestFrom

```jsx
// 3. We are ready to start printing shirt...
color$.pipe(withLatestFrom(logo$))
    .subscribe(([color, logo]) => console.log(`${color} shirt with ${logo}`));
```

![https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/MKm4pKfdSeoW5YjaTorj_withlatestfrom.gif](https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/MKm4pKfdSeoW5YjaTorj_withlatestfrom.gif)

`color` is the primary while `logo` is the secondary. At first (only once), `color` (primary) will look for `logo` (secondary). Once the `logo` (secondary) has responded, `color`(primary) will take the lead. The log will get triggered whenever the next `color` (primary) value is changed. The `logo` (secondary) value changes will not trigger the console log.

## forkJoin

```jsx
// 3. We are ready to start printing shirt...
forkJoin(color$, logo$)
    .subscribe(([color, logo]) => console.log(`${color} shirt with ${logo}`));
```

![Untitled](RxJS%20operators%20-%20zip%20combineLast%20withLatestFrom%20fo%20dafafffbeb5145f4962989289589abfd/Untitled%201.png)

In our code, both `color` and `logo` observables are not complete. We can keep pushing value by calling `.next` - that means they are not serious enough and thus they are not `final destination` of each other.
To be serious, we need to `complete` both observables.

```jsx
// 5. When the two persons(observables) ...
color$.complete();
logo$.complete();
```

![https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/OQzlcACSVekxWgkuq30A_forkjoin.gif](https://scotch-res.cloudinary.com/image/upload/q_auto:good,f_auto/media/272/OQzlcACSVekxWgkuq30A_forkjoin.gif)

There is more than one way to complete observables. There are operators that allow you to auto-complete observables when conditions are met, for example [take](https://rxjs-dev.firebaseapp.com/api/operators/take), [takeUntil](https://rxjs-dev.firebaseapp.com/api/operators/takeUntil), and [first](https://rxjs-dev.firebaseapp.com/api/operators/first). Let’s say you only want to make one shirt; you only need to know the first `color` and `logo`. In this case, you don’t care about the rest of the info that Ms. Color & Mr. Logo provide. You can make use of the `take` or `first` operator to achieve auto-complete observables once the first `color` and `logo` emit.

```jsx
// 3. We are ready to start printing shirt...
const firstColor$ = color$.pipe(take(1));
const firstLogo$ = logo$.pipe(first());

forkJoin(firstColor$, firstLogo$)
    .subscribe(([color, logo]) => console.log(`${color} shirt with ${logo}`));
```