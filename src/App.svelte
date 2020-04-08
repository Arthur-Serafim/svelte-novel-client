<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./views/Home.svelte";
  import Novel from "./views/Novel.svelte";
  import Read from "./views/Read.svelte";

  export let url = "";
</script>

<style>
  :global(body) {
    padding: 0;
  }
</style>

<div>
  <Router {url}>
    <div>
      <Route path="/" component={Home} />
      <Route path="/novel/:name" let:params>
        <Novel name={params.name} />
      </Route>
      <Route path="/:name/:chapter" let:params>
        <Read name={params.name} chapter={params.chapter} />
      </Route>
    </div>
  </Router>
</div>
