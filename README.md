# Notes by me
## Authentication vs. Authorization
<p>In simple terms, authentication is the process of verifying who a user is, while authorization is the process of verifying what they have access to.</p>
<p>Authencation - indentity of user</p>
<p>Authorization - have a/c of user, ready to access</p>

## React Authentication using Firebase Authorization:
### create firebase project, Google Analytics Overview:
<pre>https://firebase.google.com/ > Login with gmail > Get started > Add project > project name > create project > continue > select web > add nickname,no deploys  yet-Register app > next(2) > Continue to console</pre>

### sign in method setup and email verification template:
<pre>> click Authentication > Get started > Sign-in method >
Google > Enable > 
Project public-facing name+Project support email > save
Email/Password > Enable > - Go to Templates</pre>
<pre>settings > project settings > configure</pre>

### create simple authentication project, firebase config:
<pre>Go to docs > Fundamentals > Web > read how to setup firebase in project
Open project folder by vs code</pre>

```js
firebase.config.js
// Your web app's Firebase configuration
const firebaseConfig = {
    apiKey: "AIzaSyC5_5FNJoy08a0hXqybecOLRI_7s7oivac",
    authDomain: "ema-john-sample-5d577.firebaseapp.com",
    projectId: "ema-john-sample-5d577",
    storageBucket: "ema-john-sample-5d577.appspot.com",
    messagingSenderId: "413333605932",
    appId: "1:413333605932:web:2be2387f4dec489e384b89"
};

export default firebaseConfig;
```

### install firebase, initialize firebase app:
npm install firebase@8.3.0
```js
// App.js
import './App.css';
import firebase from "firebase/app";
import "firebase/auth";
// initialize Firebase
import firebaseConfig from './firebaseConfig';
firebase.initializeApp(firebaseConfig);

function App() {
  const provider = new firebase.auth.GoogleAuthProvider();
  return (
    <div className="App">
    <button>Sign-in</button>
    </div>
  );
}

export default App;
```

### sign in with google, open google login popup:
```js
// App.js
import './App.css';
import firebase from "firebase/app";
import "firebase/auth";
// initialize Firebase
import firebaseConfig from './firebaseConfig';
firebase.initializeApp(firebaseConfig);

function App() {
  const provider = new firebase.auth.GoogleAuthProvider();

    // handleSignIn function
    const handleSignIn = () => {
    // console.log("handleSignIn clicked");
    firebase.auth().signInWithPopup(provider)
    .then(res => {
      const {displayName, photoURL, email} = res.user;
      console.log(displayName, photoURL, email);
    })
  }

  return (
    <div className="App">
    <button>Sign-in</button>
    <button onClick={handleSignIn}>Sign-in</button>
    </div>
  );
}

export default App;
```

### set logged in user in state, display logged in user info:
```js
// App.js
import './App.css';
import { useState } from 'react';
import firebase from "firebase/app";
import "firebase/auth";
// initialize Firebase
import firebaseConfig from './firebaseConfig';
firebase.initializeApp(firebaseConfig);

function App() {
    const [user, setUser] = useState({
    isSignedIn : false,
    name: '',
    email: '',
    photo: ''
  });

  const provider = new firebase.auth.GoogleAuthProvider();

    // handleSignIn function
    const handleSignIn = () => {
    firebase.auth().signInWithPopup(provider)
    .then(res => {
      const {displayName, photoURL, email} = res.user;
      const signedInUser = {
        isSignedIn: true,
        name: displayName,
        email: email,
        photo: photoURL
      }
      setUser(signedInUser);
      console.log(displayName, photoURL, email);
    })
    .catch(err => {
      console.log(err);
      console.log(err.message);
    });
  }

  return (
    <div className="App">
    <button>Sign-in</button>
    <button onClick={handleSignIn}>Sign-in</button>
        {
      user.isSignedIn && <div>
         <p>Welcome! {user.name}</p>
         <p>Your email: {user.email}</p>
         <img src={user.photo} alt=""></img>
      </div>
    }
    </div>
  );
}

export default App;
```

### not google user login, signout user:
```js
// App.js
import './App.css';
import { useState } from 'react';
import firebase from "firebase/app";
import "firebase/auth";
// initialize Firebase
import firebaseConfig from './firebaseConfig';
firebase.initializeApp(firebaseConfig);

function App() {
    const [user, setUser] = useState({
    isSignedIn : false,
    name: '',
    email: '',
    photo: ''
  });

  const provider = new firebase.auth.GoogleAuthProvider();

    // handleSignIn function
    const handleSignIn = () => {
    firebase.auth().signInWithPopup(provider)
    .then(res => {
      const {displayName, photoURL, email} = res.user;
      const signedInUser = {
        isSignedIn: true,
        name: displayName,
        email: email,
        photo: photoURL
      }
      setUser(signedInUser);
      console.log(displayName, photoURL, email);
    })
    .catch(err => {
      console.log(err);
      console.log(err.message);
    });
  }

   // handleSignOut function
    const handleSignOut = () => {
    firebase.auth().signOut()
    .then(res => {
      const signedOutUser = {
        isSignedIn: false,
        name: '',
        email: '',
        photo: ''
      }
      setUser(signedOutUser);
      //console.log(res);
    })
    .catch(err => {

    })
  }

  return (
    <div className="App">
    <button>Sign-in</button>
    <button onClick={handleSignIn}>Sign-in</button>
     {
        user.isSignedIn ? <button onClick={handleSignOut}>Sign-out</button> :
         <button onClick={handleSignIn}>Sign-in</button>
     }
        {
      user.isSignedIn && <div>
         <p>Welcome! {user.name}</p>
         <p>Your email: {user.email}</p>
         <img src={user.photo} alt=""></img>
      </div>
    }
    </div>
  );
}

export default App;
```
