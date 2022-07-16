<script context="module">
import 'firebase/firestore';
import firebase from 'firebase/app';
import "firebase/auth";
import {firebaseConfig} from "../lib/firebaseConfig.svelte";
import {Button, Row, Col} from 'sveltestrap'

if (!firebase.apps.length) {
   firebase.initializeApp(firebaseConfig);
}
else {
   firebase.app();
}

var provider = new firebase.auth.GoogleAuthProvider();
export const db = firebase.firestore();

function Login() {
  firebase.auth()
  .signInWithPopup(provider)
  .then((result) => {
    /** @type {firebase.auth.OAuthCredential} */
    var credential = result.credential;

    // This gives you a Google Access Token. You can use it to access the Google API.
    var token = credential.accessToken;
    // The signed-in user info.
    var user = result.user;
    console.log(user)
    db.collection('users').doc(user.uid).set({
              email: user.email,
              name: user.displayName,
              photoUrl: user.photoURL,
              creation: user.metadata.creationTime
    });
    
  }).catch((error) => {
    // Handle Errors here.
    var errorCode = error.code;
    var errorMessage = error.message;
    // The email of the user's account used.
    var email = error.email;
    // The firebase.auth.AuthCredential type that was used.
    var credential = error.credential;
    console.log("errorCode:", errorCode, "errorMessage:", errorMessage)
  });

}
function Signup() {
  firebase.auth()
  .signInWithPopup(provider)
  .then((result) => {
    /** @type {firebase.auth.OAuthCredential} */
    var credential = result.credential;

    // This gives you a Google Access Token. You can use it to access the Google API.
    var token = credential.accessToken;
    // The signed-in user info.
    var user = result.user;
    console.log(user)
    db.collection('users').doc(user.uid).set({
              email: user.email,
              name: user.displayName,
              photoUrl: user.photoURL,
              creation: user.metadata.creationTime,
              newUser: true
    });
    
  }).catch((error) => {
    // Handle Errors here.
    var errorCode = error.code;
    var errorMessage = error.message;
    // The email of the user's account used.
    var email = error.email;
    // The firebase.auth.AuthCredential type that was used.
    var credential = error.credential;
    console.log("errorCode:", errorCode, "errorMessage:", errorMessage)
  });

}
</script>
<main>
<Row>
  <Col></Col>
<Col>
<Row>
<Col></Col>
<Col>
  <div style="margin-bottom: 5px;">
    <Button color="primary" on:click={() => Login()}>Logga in</Button>
  </div>
  <div>
    <Button color="primary" on:click={() => Signup()}>Bli Medlem</Button>
  </div>  
</Col>
<Col></Col>
</Row>
</Col>
<Col></Col>
</Row>

</main>

<style>

main {
  overflow-x: hidden; /* Hide horizontal scrollbar */
}
</style>