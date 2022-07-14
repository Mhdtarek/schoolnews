<script>
import 'firebase/firestore';
import firebase from 'firebase/app';
import "firebase/auth";

import Allposts from './Components/allPosts.svelte'
import NavBar from "./Components/navBar.svelte"
</script>
<main>
<NavBar/>
<Allposts/>
</main>
