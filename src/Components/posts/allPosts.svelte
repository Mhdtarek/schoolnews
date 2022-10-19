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
    Row,
    Badge,
  } from "sveltestrap";

  import WriteData from "./writeData.svelte";
  import firebase from "firebase/app";
  import "firebase/firestore";
  import { firebaseConfig } from "../../lib/firebaseConfig.svelte";
  import "firebase/auth";
  import { klass, role } from "../Auth.svelte";
  if (!firebase.apps.length) {
    firebase.initializeApp({});
  } else {
    firebase.app();
  }
  export const db = firebase.firestore();

  let posts = [];
  let fbposts = [];
  let testpost = [];
  let numberOfPosts = 0;
  let allPosts = true;
  allPosts = true;
  let posters = { toggle: false };

  let fullPostName = "";
  let fullPostContent = "";
  let fullPostDescription = "";
  let fullPostCreatorImage = "";
  let fullPostCreatorName = "";
  let fullPlaneringSelection = {};
  function toggle() {
    posters.toggle = !posters.toggle;
  }
  function allPostsTrue() {
    allPosts = true;
  }

  db.collection("posts")
    .get()
    .then((querySnapshot) => {
      querySnapshot.forEach((doc) => {
        // doc.data() is never undefined for query doc snapshots
        let post = { ...doc.data(), id: doc.id };
        //fbPosts = [...fbPosts, post]
        //console.log(post)
        numberOfPosts += 1;
        fbposts = [...fbposts, post];
        posts = fbposts;
        let klassposts = fbposts.filter((p) => {
          return p.klass === $klass;
        });
        let globalposts = fbposts.filter((p) => {
          return p.klass === "global";
        });
        let fullposts = [...klassposts, ...globalposts];
        posts = fullposts;
      });
    });
  testpost = fbposts;
  console.log(fbposts);

  posts = posts;
  function readMore(
    postContent,
    postName,
    postDescription,
    creatorIMG,
    creatorText,
    planeringSelection
  ) {
    allPosts = false;
    fullPostCreatorImage = creatorIMG;
    fullPostName = postName;
    fullPostContent = postContent;
    fullPostDescription = postDescription;
    fullPostCreatorName = creatorText;
    fullPlaneringSelection = planeringSelection;
  }
</script>

<main class="main">
  {#if !posters.toggle}
    {#if allPosts}
      {#if $role === "teacher"}
        <Button color="primary" style="margin: 10px 0;" on:click={toggle}>
          skapa Lärologg
        </Button>
      {/if}
      <Container>
        {#if $role === "elev"}
          <div class="marginup" />
        {/if}
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
                  <Button
                    color="primary"
                    on:click={() =>
                      readMore(
                        post.content.content,
                        post.name.name,
                        post.description.description,
                        post.userCreatorImage.$userPhotoURL,
                        post.userCreator.$userDisplayName,
                        post.planeringSelection
                      )}>Läs Mer</Button
                  >
                </CardBody>
              </Card>
            </Col>
          {/each}
        </div>
      </Container>
    {/if}
    {#if !allPosts}
      <div style="margin-top: 15px">
        <Button color="primary" on:click={() => (allPosts = true)}
          >Tillbaka</Button
        >
        <Container>
          <h2 style="margin-bottom: 15px; margin-top: 10px;">{fullPostName}</h2>
          <div color="dark" style="margin-bottom: 10px; margin-top: 10px;">
            <img
              style="border-radius: .25rem;"
              src={fullPostCreatorImage}
              alt=""
              width="32"
              height="32"
            /> <span>skapat av <b>{fullPostCreatorName}</b></span>
          </div>
          <h6>{fullPostDescription}</h6>
          <p>{fullPostContent}</p>
          {#if fullPlaneringSelection.isSelected == true}
            <Card class="mb-3">
              <CardHeader>
                <Badge pill color="secondary">planering</Badge>
                <CardTitle>{fullPlaneringSelection.name}</CardTitle>
              </CardHeader>
              <CardBody>
                <CardText>
                  {fullPlaneringSelection.description}
                </CardText>
                <Button>Läs Hela</Button>
              </CardBody>
            </Card>
          {/if}
        </Container>
      </div>
    {/if}
  {/if}
  {#if posters.toggle}
    <div class="marginup">
      <Button color="primary" style="margin: 0px 0;" on:click={toggle}>
        Gå tillbaka
      </Button>

      <WriteData />
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
    margin-top: 10px;
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
