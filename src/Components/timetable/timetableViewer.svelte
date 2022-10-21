<script>
  import {
    Accordion,
    AccordionItem,
    Column,
    Table,
    Alert,
    Button,
  } from "sveltestrap";
  import { klass, role } from "../Auth.svelte";
  import firebase from "firebase/app";
  import "firebase/firestore";
  import firebaseConfig from "../../lib/firebaseConfig.svelte";
  import TimetableWriter from "./timetableWriter.svelte";
  if (!firebase.apps.length) {
    firebase.initializeApp(firebaseConfig);
  } else {
    firebase.app();
  }
  let visible = false;
  let isWriterVisible = false;
  const db = firebase.firestore();
  const docRef = db.collection("timetables").doc(`klass_${$klass}`);
  let items = [];
  docRef
    .get()
    .then((doc) => {
      if (doc.exists) {
        console.log("Document data:", doc.data());
        items = doc.data().days;
      } else {
        // doc.data() will be undefined in this case
        console.log("No such document!");
        visible = true;
      }
    })
    .catch((error) => {
      console.log("Error getting document:", error);
      visible = true;
    });
</script>

<main>
  {#if !isWriterVisible}
    <div style="margin-top: 10px;">
      {#if $role == "teacher"}
        <Button
          color="primary"
          on:click={() => (isWriterVisible = true)}
          style="margin-bottom: 5px;">Ändra Schema</Button
        >
      {/if}
      <Alert color="primary" isOpen={visible} toggle={() => (visible = false)}>
        Din klass har ingen schema!
      </Alert>
      <Accordion>
        {#each items as day}
          <AccordionItem header={day.dayName}>
            <Table rows={day.dayItems} let:row>
              <Column header="Klass" width="8rem">
                {row.class}
              </Column>
              <Column header="Rum" width="8rem">
                {row.room}
              </Column>
              <Column header="Tid" width="8rem">
                {row.time}
              </Column>
            </Table>
          </AccordionItem>
        {/each}
      </Accordion>
    </div>
  {/if}
  {#if isWriterVisible}
    <div style="margin-top: 10px; margin-bottom: 10px;">
      <Button color="primary" on:click={() => (isWriterVisible = false)}
        >Gå tillbaka</Button
      >
    </div>
    <TimetableWriter />
  {/if}
</main>

<style></style>
