# :zap: Angular Firebase Auth Service

Builds a user authentication app using Angular 11 and google Firebase.

*** Note: to open web links in a new window use: _ctrl+click on link_**

## :page_facing_up: Table of contents

* [:zap: Angular Firebase Auth Service](#zap-angular-firebase-auth-service)
  * [:page_facing_up: Table of contents](#page_facing_up-table-of-contents)
  * [:books: General info](#books-general-info)
  * [:camera: Screenshots](#camera-screenshots)
  * [:signal_strength: Technologies](#signal_strength-technologies)
  * [:floppy_disk: Setup](#floppy_disk-setup)
  * [:computer: Code Examples](#computer-code-examples)
  * [:cool: Features](#cool-features)
  * [:clipboard: Status & To-Do List](#clipboard-status--to-do-list)
  * [:clap: Inspiration](#clap-inspiration)
  * [:envelope: Contact](#envelope-contact)

## :books: General info

* Firebase Real-time database used, rules set to test:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}


```

* This is just for development, not production, so the whole firebase module is used. This error is shown in the dev tools console: _When deploying Firebase apps to production, it is advisable to only import the individual SDK components you intend to use._

## :camera: Screenshots

![Example screenshot](./img/user-logged-in.png).

## :signal_strength: Technologies

* [Angular v11](https://angular.io/)
* [Firebase v8](https://firebase.google.com/)
* [Reactive Extensions for Javascript -RxJS v6](https://angular.io/guide/rx-library) library used for reactive programming using the observable type.
* [RxJS SwitchMap](https://www.learnrxjs.io/learn-rxjs/operators/transformation/switchmap) On each emission **the previous inner observable is cancelled and the new observable is subscribed**; switch to a new observable. Only one subscription at a time - seems ideal for login applications

## :floppy_disk: Setup

* Install dependencies with `npm i`
* Create google firebase project and paste access keys into app.module.ts
* Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files

## :computer: Code Examples

* _user.service_ file

```typescript
@Injectable({
  providedIn: 'root'
})
export class UserService {
  uid = this.afAuth.authState.pipe(
    map(authState => {
      if (!authState) {
        return null;
      } else {
        return authState.uid;
      }
    })
  );
  //test to see if already logged in
  isAdmin: Observable<boolean> = this.uid.pipe(
    switchMap(uid => {
      if (!uid) {
        return observableOf(false);
      } else {
        return this.db.object<boolean>('/admin/' +uid).valueChanges();
      }
    })
  );
  //test to see if user has specific permissions
  constructor(private afAuth: AngularFireAuth, private db: AngularFireDatabase) { }
  login() {
    this.afAuth.auth.signInWithPopup(new auth.GoogleAuthProvider());
  }
  logout() {
    this.afAuth.auth.signOut();
  }
}
```

## :cool: Features

* user login using google id or password, can be viewed on Firebase Console.
* User privileges can be revoked from the Firebase console.
* Updated to latest Angular v10 and all dependencies are the latest versions with no conflicts.

## :clipboard: Status & To-Do List

* Status: Updated and working app with a successful login storing login details in the firebase database.
* To-Do: add theme colors and functionality. A bootstrap button would be better.

## :clap: Inspiration

* [Demos With Angular: Building A User Authentication Service with Angular and Firebase](https://www.youtube.com/watch?v=mfONkAj4x94).

## :envelope: Contact

Repo created by [ABateman](https://www.andrewbateman.org) - feel free to contact me!
