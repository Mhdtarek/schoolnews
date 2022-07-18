<script>
import Allposts from './Components/allPosts.svelte'
import NavBar from "./Components/navBar.svelte"
import Auth from './Components/Auth.svelte'
import {username, userDisplayName, userCreation, userPhotoURL, role, klass, isLoggedIn} from './Components/Auth.svelte'
</script>
<main>
<NavBar/>
<Allposts/>
{username} {userDisplayName} {userPhotoURL} {userCreation}
<Auth/>
</main>
