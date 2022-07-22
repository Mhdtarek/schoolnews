<script>
    import { TabContent, TabPane, Container } from 'sveltestrap';
    import AllPosts from './posts/allPosts.svelte';
  </script>
  

  <Container>
      <TabContent>
          <TabPane tabId="alpha" tab="Alpha" active>
              <AllPosts/>
        </TabPane>
        <TabPane tabId="bravo" tab="Bravo">
            <h2>Bravo</h2>
            <img
            alt="Johnny Bravo"
            src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Johnny_Bravo_series_logo.png/320px-Johnny_Bravo_series_logo.png"
            />
        </TabPane>
        <TabPane tabId="charlie" tab="Charlie">
            <h2>Charlie</h2>
            <img
            alt="Charlie Brown"
            src="https://upload.wikimedia.org/wikipedia/en/2/22/Charlie_Brown.png"
            />
        </TabPane>
    </TabContent>
</Container>
    