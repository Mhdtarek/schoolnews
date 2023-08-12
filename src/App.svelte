<script>
  // @ts-ignore
  import Posts from "./Components/posts/allPosts.svelte";
  import Plans from "./Components/plans/plans.svelte";
  import NavBar from "./Components/navBar.svelte";
  import Auth from "./Components/Auth.svelte";
  import TimeTable from "./Components/timetable/timetableViewer.svelte";
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
    <div style="margin-top: 22px">
      <Container>
        <TabContent>
          <TabPane tabId="posts" tab="Lärologg" active>
            <Posts />
          </TabPane>
          <TabPane tabId="plans" tab="Planeringar">
            <Plans />
          </TabPane>
          <TabPane tabId="timeTable" tab="Schema">
            <TimeTable />
          </TabPane>
        </TabContent>
      </Container>
    </div>
  {/if}
  {#if !$isNotLoggedIn}
    <div>
      <Auth />
    </div>
  {/if}
</main>

<style>
</style>
