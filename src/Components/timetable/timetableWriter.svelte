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

  let updateKlass = 0;
  let timeTable = {};
  let timeTableDays = [];
  let timeTableRead = false;
  let successUpdatedTimeTable = false;
  function getTimeTable(klass) {
    db.collection("timetables")
      .doc(`klass_${klass}`)
      .get()
      .then((doc) => {
        if (doc.exists) {
          console.log("Document data:", doc.data());
          timeTableDays = doc.data().days;
          timeTable = doc.data();
          timeTableRead = true;
          updateKlass = klass;
        } else {
          // doc.data() will be undefined in this case
          console.log("No such document!");
          createTimeTable(klass);
        }
      });
  }
  async function updateTimeTable(klass) {
    db.collection("timetables")
      .doc(`klass_${klass}`)
      .update({
        days: timeTableDays,
      })
      .then(() => {
        console.log("Document successfully updated!");
        getTimeTable(klass);
      })
      .catch((error) => {
        // The document probably doesn't exist.
        console.error("Error updating document: ", error);
      });
  }
  function createTimeTable(klass) {
    db.collection("timetables")
      .doc(`klass_${klass}`)
      .set({
        days: [
          { arrayUpdate: 0, dayItems: [], dayName: "Måndag" },
          { arrayUpdate: 1, dayItems: [], dayName: "Tisdag" },
          { arrayUpdate: 2, dayItems: [], dayName: "Onsdag" },
          { arrayUpdate: 3, dayItems: [], dayName: "Torsdag" },
          { arrayUpdate: 4, dayItems: [], dayName: "Fredag" },
        ],
      })
      .then(() => {
        console.log("Document successfully written!");
      })
      .catch((error) => {
        console.error("Error writing document: ", error);
      });
  }
  function addNewTimeTableRow(dayArray) {
    let dayItems = timeTableDays[dayArray].dayItems;
    console.log(timeTableDays);
    timeTableDays[dayArray].dayItems = [
      ...dayItems,
      {
        class: "",
        room: "",
        time: "",
        arrayUpdate: timeTableDays[dayArray].dayItems.length - 1,
      },
    ];
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
        <Button
          style="margin-top: 10px;"
          on:click={() => addNewTimeTableRow(day.arrayUpdate)}
        >
          Skapa en ny tid
        </Button>
      </AccordionItem>
    {/each}
  </Accordion>
  <div class="p-3 mb-3 d-flex justify-content-center ">
    {#if !timeTableRead}
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
    {/if}
    {#if timeTableRead}
      <Button on:click={() => updateTimeTable(updateKlass)}
        >Uppdatera schema</Button
      >
    {/if}
  </div>
</main>

<style>
</style>
