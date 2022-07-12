<script>
  import {
    Button,
    Card,
    CardBody,
    CardFooter,
    CardHeader,
    CardSubtitle,
    CardText,
    CardTitle,
    Col,
    Container,
    Row,
  } from 'sveltestrap';


import firebase from 'firebase/app';
import 'firebase/firestore';
import {firebaseConfig} from "../lib/firebaseConfig.svelte";
import WriteData from './writeData.svelte'

firebase.initializeApp(firebaseConfig);
export const db = firebase.firestore();

let posts = [];
let fbposts = []
let numberOfPosts = 0
export let allPosts = true
allPosts = true

let fullPostName = ''
let fullPostContent = ''
let fullPostDescription = ''




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
{#if allPosts}
  <Container>
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
            <Button on:click={() => readMore(post.content.content, post.name.name, post.description.description)}>Läs Mer</Button>
          </CardBody>
        </Card>
      </Col>
      {/each}
    </div>
  </Container>
{/if}
{#if !allPosts}
<Container>
{fullPostName}
{fullPostDescription}
<p>{fullPostContent}</p>
</Container>
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
</style>