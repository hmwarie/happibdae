<script>
  import './app.css';
  import Router from 'svelte-spa-router';
  import Landing from './routes/Landing.svelte';
  import MomMessage from './routes/MomMessage.svelte';
  import SisterMessage from './routes/SisterMessage.svelte';

  const routes = {
    '/': Landing,
    '/mom-message': MomMessage,
    '/sister-message': SisterMessage
  };
</script>

<div class="app">
  <Router {routes} />
</div>

<style>
  .app {
    min-height: 100vh;
    width: 100%;
  }

  :global(body) {
    margin: 0;
    padding: 0;
    min-height: 100vh;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
</style> 