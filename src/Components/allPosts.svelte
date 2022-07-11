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
TabContent
  } from 'sveltestrap';


import firebase from 'firebase/app';
import 'firebase/firestore';
import {firebaseConfig} from "../lib/firebaseConfig.svelte";
import { loop_guard } from 'svelte/internal';

firebase.initializeApp(firebaseConfig);
export const db = firebase.firestore();

let posts = [];
let fbposts = []
let numberOfPosts = 0
let allPosts = true
allPosts = true
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
console.log(postContent)
allPosts = false

}


</script>

<main>
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



</Container>
{/if}


</main>

<style>
.gridcon {
display: grid;
grid-template-columns: 1fr 1fr 1fr 1fr;
column-gap: 10px;
row-gap: 15px;

}
</style>