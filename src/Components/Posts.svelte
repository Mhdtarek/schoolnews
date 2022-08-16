<script>
    import { TabContent, TabPane, Container } from 'sveltestrap';
    import AllPosts from './posts/allPosts.svelte';
    import firebase from 'firebase/app';
    import 'firebase/firestore';
    import "firebase/auth";
    import {writable} from 'svelte/store'
    import {klass} from './Auth.svelte'
    if (!firebase.apps.length) {
    firebase.initializeApp({});
    }
    else {
       firebase.app();
    }

    export const db = firebase.firestore();


let posts = [];
let fbposts = []
let testpost = [];
let numberOfPosts = 0
let allPosts = true
allPosts = true
let posters = { toggle: false };



let fullPostName = ''
let fullPostContent = ''
let fullPostDescription = ''
let fullPostCreatorImage = ''
let fullPostCreatorName = ''


  </script>
      <AllPosts/>
