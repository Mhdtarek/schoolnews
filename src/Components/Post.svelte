<script>
    import { TabContent, TabPane, Container } from 'sveltestrap';
    import AllPosts from './posts/allPosts.svelte';
  </script>
  

  <Container>
      <TabContent>
          <TabPane tabId="alla" tab="Alla" active>
              <AllPosts/>
        </TabPane>

    </TabContent>
</Container>
    
