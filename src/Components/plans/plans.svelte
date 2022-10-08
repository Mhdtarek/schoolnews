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
  } from "sveltestrap";

  // @ts-ignore
  import WriteData from "./writePlan.svelte";
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

  let plans = [];
  let fbplans = [];
  let testplan = [];
  let numberOfplans = 0;
  let allplans = true;
  allplans = true;
  let planers = { toggle: false };

  let fullplanName = "";
  let fullplanContent = "";
  let fullplanDescription = "";
  let fullplanCreatorImage = "";
  let fullplanCreatorName = "";
  function toggle() {
    planers.toggle = !planers.toggle;
  }
  function allplansTrue() {
    allplans = true;
  }

  db.collection("planeringar")
    .get()
    .then((querySnapshot) => {
      querySnapshot.forEach((doc) => {
        // doc.data() is never undefined for query doc snapshots
        let plan = { ...doc.data(), id: doc.id };
        //fbplans = [...fbplans, plan]
        //console.log(plan)
        numberOfplans += 1;
        fbplans = [...fbplans, plan];
        plans = fbplans;
        let klassplans = fbplans.filter((p) => {
          return p.klass === $klass;
        });
        let globalplans = fbplans.filter((p) => {
          return p.klass === "global";
        });
        let fullplans = [...klassplans, ...globalplans];
        plans = fullplans;
      });
    });
  testplan = fbplans;
  console.log(fbplans);

  plans = plans;
  function readMore(
    planContent,
    planName,
    planDescription,
    creatorIMG,
    creatorText
  ) {
    allplans = false;
    fullplanCreatorImage = creatorIMG;
    fullplanName = planName;
    fullplanContent = planContent;
    fullplanDescription = planDescription;
    fullplanCreatorName = creatorText;
  }
</script>

<main class="main">
  {#if !planers.toggle}
    {#if allplans}
      <Container>
        {#if $role === "teacher"}
          <Button color="primary" style="margin: 10px 0;" on:click={toggle}>
            skapa planering
          </Button>
        {/if}
        {#if $role === "elev"}
          <div class="marginup" />
        {/if}
        <div class="gridcon">
          {#each plans as plan}
            <Col>
              <Card>
                <CardHeader>
                  <CardTitle>{plan.name.name}</CardTitle>
                </CardHeader>
                <CardBody>
                  <CardText>
                    {plan.description.description}
                  </CardText>
                  <Button
                    color="primary"
                    on:click={() =>
                      readMore(
                        plan.content.content,
                        plan.name.name,
                        plan.description.description,
                        plan.userCreatorImage.$userPhotoURL,
                        plan.userCreator.$userDisplayName
                      )}>Läs Mer</Button
                  >
                </CardBody>
              </Card>
            </Col>
          {/each}
        </div>
      </Container>
    {/if}
    {#if !allplans}
      <div style="margin-top: 15px">
        <Container>
          <Button color="primary" on:click={() => (allplans = true)}
            >Tillbaka</Button
          >
          <h2>{fullplanName}</h2>
          <div color="dark" style="margin-bottom: 10px;">
            <img
              style="border-radius: .25rem;"
              src={fullplanCreatorImage}
              alt=""
              width="32"
              height="32"
            /> <span>skapat av {fullplanCreatorName}</span>
          </div>
          <h5>{fullplanDescription}</h5>
          <p>{fullplanContent}</p>
        </Container>
      </div>
    {/if}
  {/if}
  {#if planers.toggle}
    <div class="marginup">
      <Container>
        <Button color="primary" style="margin: 0px 0;" on:click={toggle}>
          Gå tillbaka
        </Button>
      </Container>

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
