<script lang="ts">
  import {
    FormGroup,
    Input,
    Label,
    Col,
    Row,
    Button,
    Container,
    Dropdown,
    DropdownMenu,
    DropdownItem,
    DropdownToggle,
    Card,
    CardBody,
    CardHeader,
    CardText,
    CardTitle,
    Alert,
  } from "sveltestrap";
  import firebase from "firebase/app";
  import "firebase/firestore";
  import { firebaseConfig } from "../../lib/firebaseConfig.svelte";
  import { userDisplayName, userPhotoURL, username } from "../Auth.svelte";

  if (!firebase.apps.length) {
    firebase.initializeApp(firebaseConfig);
  } else {
    firebase.app();
  }
  let name = "";
  let description = "";
  let content = "";
  let isOpen = false;
  let isCollapseOpen = false;
  let docSuccess = false;
  let klass;
  let plans = [];
  let fbplans = [];
  let visible = false;

  let planeringSelection = {
    isSelected: false,
    content: "",
    name: "",
    description: "",
    creator: "",
    creatorImage: "",
  };

  export const db = firebase.firestore();

  function post() {
    let userDisplayName2 = { $userDisplayName };
    let userPhotoURL2 = { $userPhotoURL };
    let userId2 = { $username };
    db.collection("posts")
      .add({
        name: { name },
        description: { description },
        content: { content },
        userCreator: userDisplayName2,
        userCreatorImage: userPhotoURL2,
        userId: userId2,
        klass: klass,
        planeringSelection: planeringSelection,
      })
      .then((docRef) => {
        console.log("Document written with ID: ", docRef.id);
        docSuccess = true;
      })
      .catch((error) => {
        console.error("Error adding document: ", error);
      });
  }
  function select(content, name, description, creator, creatorImage) {
    isCollapseOpen = false;
    visible = true;
    planeringSelection = {
      isSelected: true,
      description: description,
      creator: creator,
      creatorImage: creatorImage,
      name: name,
      content: content,
    };
  }

  db.collection("planeringar")
    .get()
    .then((querySnapshot) => {
      querySnapshot.forEach((doc) => {
        // doc.data() is never undefined for query doc snapshots
        let plan = { ...doc.data(), id: doc.id };
        //fbplans = [...fbplans, plan]
        //console.log(plan)
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
</script>

<main>
  {#if !docSuccess}
    <Alert
      style="margin-top: 10px;"
      color="info"
      isOpen={visible}
      toggle={() => (visible = false)}
    >
      planering {planeringSelection.name} är vald.
    </Alert>
    <Row>
      <Col sm="12" md={{ size: 6, offset: 3 }}>
        <FormGroup>
          <Label>Namn</Label>
          <Input type="text" bind:value={name} />
        </FormGroup>
        <FormGroup>
          <Label>Beskrivning</Label>
          <Input type="text" bind:value={description} />
        </FormGroup>
        <FormGroup>
          <Label>Innehål</Label>
          <Input
            bind:value={content}
            style="height: 150px;"
            rows={1}
            type="textarea"
          />
        </FormGroup>

        <Row>
          <Col xs="3">
            <Dropdown {isOpen} toggle={() => (isOpen = !isOpen)}>
              <DropdownToggle caret>välj klass</DropdownToggle>
              <DropdownMenu dark>
                <DropdownItem
                  color="primary"
                  on:click={() => (klass = "global")}>Alla Klasser</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 1)}
                  >klass 1</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 2)}
                  >klass 2</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 3)}
                  >klass 3</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 4)}
                  >klass 4</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 5)}
                  >klass 5</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 6)}
                  >klass 6</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 7)}
                  >klass 7</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 8)}
                  >klass 8</DropdownItem
                >
                <DropdownItem color="primary" on:click={() => (klass = 9)}
                  >klass 9</DropdownItem
                >
              </DropdownMenu>
            </Dropdown>
          </Col>
          <Col xs="9">
            <div>
              <Button
                color="secondary"
                on:click={() => (isCollapseOpen = !isCollapseOpen)}
              >
                länka en planering
              </Button>
            </div>
          </Col>
        </Row>
        {#if isCollapseOpen == true}
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
                        select(
                          plan.content.content,
                          plan.name.name,
                          plan.description.description,
                          plan.userCreatorImage.$userPhotoURL,
                          plan.userCreator.$userDisplayName
                        )}>Välj Planering</Button
                    >
                  </CardBody>
                </Card>
              </Col>
            {/each}
          </div>{/if}
        <Button style="margin-top: 10px;" color="primary" on:click={post}
          >Lägg</Button
        >
      </Col>
    </Row>
  {/if}
  {#if docSuccess}
    <Container>
      <div class="bg-success">skapat!</div>
    </Container>
  {/if}
</main>

<style>
  main {
    overflow-x: hidden; /* Hide horizontal scrollbar */
  }
  .bg-success {
    border-radius: 0.25rem;
    margin-top: 10px;
    padding: 15px;
    background-color: rgb(78, 134, 91);
    color: whitesmoke;
    text-align: center;
  }
  @media only screen and (max-width: 900px) {
    .gridcon {
      grid-template-columns: 1fr;
    }
  }
  .gridcon {
    margin-top: 15px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    column-gap: 10px;
    row-gap: 15px;
  }
</style>
