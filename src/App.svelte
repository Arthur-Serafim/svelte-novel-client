<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./views/Home.svelte";

  export let url = "";
</script>

<style>
  :global(body) {
    padding: 0;
    background: #f5f6fc;
  }
</style>

<div>
  <Router {url}>
    <div>
      <Route path="/" component={Home} />
    </div>
  </Router>
</div>
