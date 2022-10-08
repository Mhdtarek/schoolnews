<script>
  // @ts-ignore
  import Posts from "./Components/posts/allPosts.svelte";
  import Plans from "./Components/plans/plans.svelte";
  import NavBar from "./Components/navBar.svelte";
  import Auth from "./Components/Auth.svelte";
  import {
    username,
    userDisplayName,
    userCreation,
    userPhotoURL,
    role,
    klass,
    isLoggedIn,
    isNotLoggedIn,
  } from "./Components/Auth.svelte";
  import { TabContent, TabPane, Container } from "sveltestrap";
</script>

<main>
  <NavBar />
  {#if $isLoggedIn}
    <div style="margin-top: 15px">
      <Container>
        <TabContent>
          <TabPane tabId="posts" tab="Posts" active>
            <Posts />
          </TabPane>
          <TabPane tabId="plans" tab="Planeringar" active>
            <Plans />
          </TabPane>
        </TabContent>
      </Container>
    </div>
  {/if}
  <div style="display: none;">
    <img src={$userPhotoURL} alt="" />
    <p>{$username}</p>
    <p>{$userDisplayName}</p>
    <p>{$userPhotoURL}</p>
    <p>{$userCreation}</p>
    <p>{$role}</p>
    <p>{$klass}</p>
  </div>
  {#if !$isNotLoggedIn}
    <div>
      <Auth />
    </div>
  {/if}
</main>

<style>
</style>
