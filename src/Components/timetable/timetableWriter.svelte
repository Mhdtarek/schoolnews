<script>
  import {
    Accordion,
    AccordionItem,
    Column,
    Table,
    Alert,
    Button,
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
    Input,
    Label,
    Row,
    Col,
  } from "sveltestrap";
  import { klass, role } from "../Auth.svelte";
  import firebase from "firebase/app";
  import "firebase/firestore";
  import firebaseConfig from "../../lib/firebaseConfig.svelte";

  if (!firebase.apps.length) {
    firebase.initializeApp(firebaseConfig);
  } else {
    firebase.app();
  }
  const db = firebase.firestore();

  let timeTable = {};
  let timeTableDays = [];

  function getTimeTable(klass) {
    db.collection("timetables")
      .doc(`klass_${klass}`)
      .get()
      .then((doc) => {
        if (doc.exists) {
          console.log("Document data:", doc.data());
          timeTableDays = doc.data().days;
          timeTable = doc.data();
          console.log(timeTableDays);
        } else {
          // doc.data() will be undefined in this case
          console.log("No such document!");
        }
      });
  }

  function createTimeTable() {}
  function addNewTimeTable(arrayNumber) {
    let dayItems = timeTableDays[0].dayItems;
    console.log(timeTableDays);
    timeTableDays[0].dayItems = [
      ...dayItems,
      {
        class: "",
        room: "",
        time: "",
        arrayUpdate: timeTableDays[0].dayItems.length - 1,
      },
    ];
  }
  function test() {
    console.log(timeTableDays);
  }
</script>

<main>
  <Accordion>
    {#each timeTableDays as day}
      <AccordionItem header={day.dayName}>
        {#each day.dayItems as row}
          <Row style="margin-top: 5px;">
            <Col>
              <Label>Klass</Label>
              <Input type="text" bind:value={row.class} />
            </Col>
            <Col>
              <Label>Rum</Label>
              <Input type="text" bind:value={row.room} />
            </Col>
            <Col>
              <Label>Tid</Label>
              <Input
                value={row.time}
                type="time"
                name="time"
                id="exampleTime"
                placeholder="time placeholder"
              />
            </Col>
          </Row>
        {/each}
        <Button style="margin-top: 10px;" on:click={() => addNewTimeTable()}>
          Skapa en ny tid
        </Button>
      </AccordionItem>
    {/each}
  </Accordion>
  <div class="p-3 mb-3 d-flex justify-content-center ">
    <Dropdown style=" overflow: inherit;">
      <DropdownToggle caret>välj klass</DropdownToggle>
      <DropdownMenu>
        <DropdownItem color="primary" on:click={() => getTimeTable(1)}
          >klass 1</DropdownItem
        >
        <DropdownItem color="primary" on:click={() => getTimeTable(2)}
          >klass 2</DropdownItem
        >
        <DropdownItem color="primary" on:click={() => getTimeTable(3)}
          >klass 3</DropdownItem
        >
        <DropdownItem color="primary" on:click={() => getTimeTable(4)}
          >klass 4</DropdownItem
        >
        <DropdownItem color="primary" on:click={() => getTimeTable(5)}
          >klass 5</DropdownItem
        >
        <DropdownItem color="primary" on:click={() => getTimeTable(6)}
          >klass 6</DropdownItem
        >
        <DropdownItem color="primary" on:click={() => getTimeTable(7)}
          >klass 7</DropdownItem
        >
        <DropdownItem color="primary" on:click={() => getTimeTable(8)}
          >klass 8</DropdownItem
        >
        <DropdownItem color="primary" on:click={() => getTimeTable(8)}
          >klass 8</DropdownItem
        >
      </DropdownMenu>
    </Dropdown>
  </div>
</main>

<style>
</style>
