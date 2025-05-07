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

  // Configure router with base URL
  const baseUrl = '/happibdae';
  
  // Handle 404s
  const handleRoute = (location) => {
    if (!routes[location.pathname]) {
      window.location.href = baseUrl + '/';
    }
  };
</script>

<div class="app">
  <Router {routes} base={baseUrl} on:route={handleRoute} />
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