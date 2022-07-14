<script>
  import {
    Button,
    Card,
    CardBody,
    CardHeader,
    CardText,
    CardTitle,
    Col,
    Container,
    Icon,
    Row
  } from 'sveltestrap';


import firebase from 'firebase/app';
import 'firebase/firestore';
import {firebaseConfig} from "../lib/firebaseConfig.svelte";
import WriteData from './writeData.svelte'
import "firebase/auth";

var provider = new firebase.auth.GoogleAuthProvider();
firebase.initializeApp(firebaseConfig);
export const db = firebase.firestore();


let posts = [];
let fbposts = []
let numberOfPosts = 0
let allPosts = true
allPosts = true
let posters = { toggle: false };



let fullPostName = ''
let fullPostContent = ''
let fullPostDescription = ''

function toggle() {
    posters.toggle = !posters.toggle;
}
function allPostsTrue() {
  allPosts = true
}

function firebaseAuth() {
  firebase.auth()
  .signInWithPopup(provider)
  .then((result) => {
    /** @type {firebase.auth.OAuthCredential} */
    var credential = result.credential;

    // This gives you a Google Access Token. You can use it to access the Google API.
    var token = credential.accessToken;
    // The signed-in user info.
    var user = result.user;
    console.log(user, token)
    // ...
  }).catch((error) => {
    // Handle Errors here.
    var errorCode = error.code;
    var errorMessage = error.message;
    // The email of the user's account used.
    var email = error.email;
    // The firebase.auth.AuthCredential type that was used.
    var credential = error.credential;
    // ...
  });

}


db.collection("posts")
.get()
.then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
            // doc.data() is never undefined for query doc snapshots
            let post = {...doc.data(), id: doc.id}
            //fbPosts = [...fbPosts, post]
            //console.log(post)
            numberOfPosts += 1
            fbposts = [...fbposts, post]
            posts = fbposts
        });
        console.log(posts)        
})
let postsBy4 = numberOfPosts / 4

function readMore(postContent, postName, postDescription) {
allPosts = false
fullPostName = postName
fullPostContent = postContent
fullPostDescription = postDescription

}


</script>

<main class="main">
<Row><Button color="primary" on:click={() => firebaseAuth()}>auth</Button></Row>
{#if !posters.toggle}
{#if allPosts}
  <Container>
    <Button color="primary" style="margin: 10px 0;"  on:click={toggle}>
      skapa post
    </Button>
    <div class="gridcon">
      {#each posts as post}
      <Col>
        <Card>
          <CardHeader>
            <CardTitle>{post.name.name}</CardTitle>
          </CardHeader>
          <CardBody>
            <CardText>
              {post.description.description}
            </CardText>
            <Button color="primary" on:click={() => readMore(post.content.content, post.name.name, post.description.description)}>Läs Mer</Button>
          </CardBody>
        </Card>
      </Col>
      {/each}
    </div>
  </Container>
{/if}
{#if !allPosts}
<div style="margin-top: 15px">
  <Container>
    <Button color="dark" on:click={() => allPosts = true}>Tillbaka</Button>
    <h2>{fullPostName}</h2>
    <h5>{fullPostDescription}</h5>
    <p>{fullPostContent}</p>
  </Container>
</div>
{/if}
{/if}
{#if posters.toggle}
<div class="marginup">
  <WriteData/>
</div>
{/if}

</main>

<style>
.main {
  white-space: pre-line;
}
.gridcon {
display: grid;
grid-template-columns: 1fr 1fr 1fr 1fr;
column-gap: 10px;
row-gap: 15px;
}
.marginup {
  margin-top: 30px;
}
@media only screen and (max-width: 900px) {
.gridcon {
  grid-template-columns: 1fr 1fr;
}  
}
@media only screen and (max-width: 1500) {
.gridcon {
  grid-template-columns: 1fr 1fr 1fr;
}
}


</style>