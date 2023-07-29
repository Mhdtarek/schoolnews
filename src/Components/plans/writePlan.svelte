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
    Collapse,
    Card,
    CardBody,
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

  export const db = firebase.firestore();

  function plan() {
    let userDisplayName2 = { $userDisplayName };
    let userPhotoURL2 = { $userPhotoURL };
    let userId2 = { $username };
    db.collection("planeringar")
      .add({
        name: { name },
        description: { description },
        content: { content },
        userCreator: userDisplayName2,
        userCreatorImage: userPhotoURL2,
        userId: userId2,
        klass: klass,
      })
      .then((docRef) => {
        console.log("Document written with ID: ", docRef.id);
        docSuccess = true;
      })
      .catch((error) => {
        console.error("Error adding document: ", error);
      });
  }
</script>

<main>
  {#if !docSuccess}
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
          <Label>Innehåll</Label>
          <Input
            bind:value={content}
            style="height: 150px;"
            rows={1}
            type="textarea"
          />
        </FormGroup>
        <Dropdown {isOpen} toggle={() => (isOpen = !isOpen)}>
          <DropdownToggle caret>välj klass</DropdownToggle>
          <DropdownMenu dark>
            <DropdownItem color="primary" on:click={() => (klass = "global")}
              >Alla Klasser</DropdownItem
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
        <Button style="margin-top: 5px;" color="primary" on:click={plan}
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
</style>
