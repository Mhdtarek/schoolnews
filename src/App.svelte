<script>
import Allposts from './Components/allPosts.svelte'
import NavBar from "./Components/navBar.svelte"
import Auth from './Components/Auth.svelte'
import {username, userDisplayName, userCreation, userPhotoURL, role, klass, isLoggedIn, isNotLoggedIn} from './Components/Auth.svelte'
</script>
<main>
<div>
<NavBar/>



{#if $isLoggedIn}
<Allposts/>
{/if}
<div>
    <img src={$userPhotoURL} alt="">
    <p>{$username}</p>
    <p>{$userDisplayName}</p>
    <p>{$userPhotoURL} </p>
    <p>{$userCreation}</p>
    <p>{$role}</p>
    <p>{$klass}</p>
</div>
{#if !$isNotLoggedIn}
    <div>  
        <Auth/>
    </div>
{/if}
</main>
<style>

    
</style>
