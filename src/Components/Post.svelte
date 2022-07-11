<script>
import {Button, Container} from 'sveltestrap'
import AllPosts from './allposts.svelte'
import Writedata from './writeData.svelte'
let user = { loggedIn: false };

function toggle() {
    user.loggedIn = !user.loggedIn;
}


</script>
<main>
{#if !user.loggedIn}
<Container>
	<Button style="margin: 10px 0;"  on:click={toggle}>
		skapa post
	</Button>
</Container>
    <AllPosts/>
{/if}
{#if user.loggedIn}
    <Writedata/>
{/if}

</main>