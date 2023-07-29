<script context="module">
  import "firebase/firestore";
  import firebase from "firebase/app";
  import "firebase/auth";
  import { firebaseConfig } from "../lib/firebaseConfig.svelte";
  import {
    Button,
    Row,
    Col,
    Container,
    Modal,
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
    Icon,
    Toast,
    ToastBody,
    ToastHeader,
  } from "sveltestrap";
  import { writable } from "svelte/store";
  import {} from "sveltestrap";

  if (!firebase.apps.length) {
    firebase.initializeApp(firebaseConfig);
  } else {
    firebase.app();
  }
  var provider = new firebase.auth.GoogleAuthProvider();
  export const db = firebase.firestore();
  export let isLoggedIn = writable(false);
  export let isNotLoggedIn = writable(false);

  export const username = writable(" ");
  export const userDisplayName = writable("");
  export const userPhotoURL = writable("");
  export const userCreation = writable("");
  export const role = writable("");
  export const klass = writable("");

  // @ts-ignore

  function Login() {
    firebase
      .auth()
      .signInWithPopup(provider)
      .then((result) => {
        /** @type {firebase.auth.OAuthCredential} */
        var credential = result.credential;

        // This gives you a Google Access Token. You can use it to access the Google API.
        var token = credential.accessToken;
        // The signed-in user info.
        var user = result.user;
        //checks if account exists in database otherwise it asks you to sign up
        db.collection("users")
          .doc(user.uid)
          .get()
          .then((doc) => {
            if (doc.exists) {
              username.set(user.uid);
              userDisplayName.set(user.displayName);
              userPhotoURL.set(user.photoURL);
              userCreation.set(user.metadata.creationTime);
              role.set(doc.data().role);
              klass.set(doc.data().klass);
              isLoggedIn.set(true);
              isNotLoggedIn.set(true);

              console.log("logged in!");
            } else {
              console.log("skapa konto först!");
            }
          })
          .catch((error) => {
            console.log("Error collection getting document:", error);
          });
      })
      .catch((error) => {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // The email of the user's account used.
        var email = error.email;
        // The firebase.auth.AuthCredential type that was used.
        var credential = error.credential;
        console.log("errorCode:", errorCode, "errorMessage:", errorMessage);
      });
  }
</script>

<script>
  let signupUserId;
  let signupUserEmail = "";
  let signupUserDisplayName = "";
  let signupUserphotourl = "";
  let signupUserCreationDate = "";
  let klass = 0;
  let signupRole = "elev";

  if (!firebase.apps.length) {
    firebase.initializeApp(firebaseConfig);
  } else {
    firebase.app();
  }

  export const db = firebase.firestore();

  let authToggle = false;
  let signupSuccess = false;

  function Signup() {
    firebase
      .auth()
      .signInWithPopup(provider)
      .then((result) => {
        /** @type {firebase.auth.OAuthCredential} */
        var credential = result.credential;

        // This gives you a Google Access Token. You can use it to access the Google API.
        var token = credential.accessToken;
        // The signed-in user info.
        var user = result.user;
        console.log(user);

        signupUserEmail = user.email;
        signupUserDisplayName = user.displayName;
        signupUserId = user.uid;
        signupUserphotourl = user.photoURL;
        signupUserCreationDate = user.metadata.creationTime;
        signupRole = "elev";

        authToggle = true;
      })
      .catch((error) => {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // The email of the user's account used.
        var email = error.email;
        // The firebase.auth.AuthCredential type that was used.
        var credential = error.credential;
        console.log("errorCode:", errorCode, "errorMessage:", errorMessage);
      });
  }
  function addAcc() {
    db.collection("users").doc(signupUserId).set({
      email: signupUserEmail,
      name: signupUserDisplayName,
      photoUrl: signupUserphotourl,
      creation: signupUserCreationDate,
      role: signupRole,
      klass: klass,
    });
    signupSuccess = true;
  }
</script>

<main>
  {#if !signupSuccess}
    {#if !authToggle}
      <div class="center-items">
        <Row>
          <Col />
          <Col>
            <div class="p-3 bg-primary mb-3 d-flex justify-content-center ">
              <Toast class="me-1">
                <ToastHeader
                  ><span class="text-center">skapa konto eller logga in</span
                  ></ToastHeader
                >
                <ToastBody>
                  <div
                    style="margin-bottom: 5px;"
                    class="d-flex justify-content-center"
                  >
                    <Button color="primary" on:click={() => Login()}
                      ><Icon name="google" /> Logga in</Button
                    >
                  </div>
                  <div class="d-flex justify-content-center">
                    <Button color="primary" on:click={() => Signup()}>
                      <Icon name="google" /> Bli Medlem
                    </Button>
                  </div>
                </ToastBody>
              </Toast>
            </div>
          </Col>
          <Col />
        </Row>
      </div>
    {/if}
    {#if authToggle}
      <div class="center-items">
        <Row>
          <Col />
          <Col>
            <div class="p-3 bg-primary mb-3 d-flex justify-content-center ">
              <Toast class="me-1">
                <ToastHeader
                  ><span class="text-center">skapa konto eller logga in</span
                  ></ToastHeader
                >
                <ToastBody>
                  <div
                    style="margin-bottom: 5px;"
                    class="d-flex justify-content-center"
                  >
                    <Dropdown style=" overflow: inherit;">
                      <DropdownToggle caret>välj klass</DropdownToggle>
                      <DropdownMenu>
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 1)}>klass 1</DropdownItem
                        >
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 2)}>klass 2</DropdownItem
                        >
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 3)}>klass 3</DropdownItem
                        >
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 4)}>klass 4</DropdownItem
                        >
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 5)}>klass 5</DropdownItem
                        >
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 6)}>klass 6</DropdownItem
                        >
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 7)}>klass 7</DropdownItem
                        >
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 8)}>klass 8</DropdownItem
                        >
                        <DropdownItem
                          color="primary"
                          on:click={() => (klass = 9)}>klass 9</DropdownItem
                        >
                      </DropdownMenu>
                    </Dropdown>
                  </div>
                  <div class="d-flex justify-content-center">
                    <Button color="success" on:click={() => addAcc()}>
                      skapa din konto
                    </Button>
                  </div>
                </ToastBody>
              </Toast>
            </div>
          </Col>
          <Col />
        </Row>
      </div>
    {/if}
  {/if}
  {#if signupSuccess}
    <div class="center-items">
      <Row>
        <Col />
        <Col>
          <div class="p-3 bg-primary mb-3 d-flex justify-content-center ">
            <Toast class="me-1">
              <ToastHeader>konto är skapat</ToastHeader>
              <ToastBody>
                <h4 class="text-center">skapat!</h4>
                <div
                  style="margin-bottom: 5px;"
                  class="d-flex justify-content-center"
                />
              </ToastBody>
            </Toast>
          </div>
        </Col>
        <Col />
      </Row>
    </div>
  {/if}
</main>

<style>
  main {
    overflow-x: hidden; /* Hide horizontal scrollbar */
    height: 94vh;
    background-color: rgb(13, 110, 253);
    display: grid;
  }
  .center-items {
    place-items: center;
  }
</style>
