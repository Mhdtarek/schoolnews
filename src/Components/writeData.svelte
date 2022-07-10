<script lang="ts">
import { FormGroup, Input, Label, Col, Row } from 'sveltestrap';
import firebase from 'firebase/app';
import 'firebase/firestore';
import {firebaseConfig} from "../lib/firebaseConfig.svelte";

if (!firebase.apps.length) {
   firebase.initializeApp({});
}
else {
   firebase.app();
}


export const db = firebase.firestore();

    function post(){
    db.collection("posts").add({
    name: "Tokyo",
    content: "Tokyo in japan",
    rating: 10
    })
    .then((docRef) => {
        console.log("Document written with ID: ", docRef.id);
    })
    .catch((error) => {
        console.error("Error adding document: ", error);
    });
    }

let inputValue = '';

</script>

<main>
<Row>
    <Col sm="12" md={{ size: 6, offset: 3 }}>
        <FormGroup>
            <Label>This textarea resizes as you type</Label>
            <Input style="height: 150px;" rows={1} type="textarea"/>
        </FormGroup>
</Col>
</Row>


</main>
<style>
main {
  overflow-x: hidden; /* Hide horizontal scrollbar */
}
</style>