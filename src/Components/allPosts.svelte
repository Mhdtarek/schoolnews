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
    Row
  } from 'sveltestrap';


import firebase from 'firebase/app';
import 'firebase/firestore';
import {firebaseConfig} from "../lib/firebaseConfig.svelte";

firebase.initializeApp(firebaseConfig);
export const db = firebase.firestore();

let posts = [];
let fbposts = []
let numberOfPosts = 0

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


</script>

<main>
  <Container>
    <div class="gridcon">
      {#each posts as post}
      <Col>
        <Card>
          <CardHeader>
            <CardTitle>{post.name}</CardTitle>
          </CardHeader>
          <CardBody>
            <CardText>
              {post.content}
              work
              <p>rating: {post.rating}</p>
              
            </CardText>
          </CardBody>
        </Card>
      </Col>
      {/each}
    </div>
  </Container>
</main>

<style>
.gridcon {
display: grid;
grid-template-columns: 1fr 1fr 1fr 1fr;
column-gap: 10px;
row-gap: 15px;

}
</style>