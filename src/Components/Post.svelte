<script>
import {Button, Container, FormGroup} from 'sveltestrap'
import AllPosts from './allPosts.svelte'
import Writedata from './writeData.svelte'
let user = { loggedIn: false };

function toggle() {
    user.loggedIn = !user.loggedIn;
}


</script>
<main>

{#if !user.loggedIn}
<Container>
	<Button color="primary" style="margin: 10px 0;"  on:click={toggle}>
		skapa post
	</Button>
</Container>
    <AllPosts/>
{/if}
{#if user.loggedIn}
    <Writedata/>
{/if}

</main>