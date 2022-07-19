<script lang="ts">
import { FormGroup, Input, Label, Col, Row, Button } from 'sveltestrap';
import firebase from 'firebase/app';
import 'firebase/firestore';
import {firebaseConfig} from "../lib/firebaseConfig.svelte";
import {userDisplayName, userPhotoURL, username} from './Auth.svelte'


if (!firebase.apps.length) {
   firebase.initializeApp(firebaseConfig);
}
else {
   firebase.app();
}
let name = '';
let description = '';
let content = '';

export const db = firebase.firestore();

    function post(){
    let userDisplayName2 = {$userDisplayName}
    let userPhotoURL2 = {$userPhotoURL}
    let userId2 = {$username}
    db.collection("posts").add({
    name: {name},
    description: {description},
    content: {content},
    userCreator: userDisplayName2,
    userCreatorImage: userPhotoURL2,
    userId: userId2,
    })
    .then((docRef) => {
        console.log("Document written with ID: ", docRef.id);
    })
    .catch((error) => {
        console.error("Error adding document: ", error);
    });
    }



</script>

<main>
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
            <Input bind:value={content} style="height: 150px;" rows={1} type="textarea"/>
        </FormGroup>
        <Button on:click={post}>Lägg</Button>
</Col>
</Row>


</main>
<style>
main {
  overflow-x: hidden; /* Hide horizontal scrollbar */
}
</style>