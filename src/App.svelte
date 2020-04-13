<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./views/Home.svelte";
  import Novel from "./views/Novel.svelte";
  import Read from "./views/Read.svelte";
  import Featured from "./views/Featured.svelte";
  import Completed from "./views/Completed.svelte";
  import Category from "./views/Category.svelte";

  export let url = "";
</script>

<style>
  :global(body) {
    padding: 0;
    font-family: Merriweather;
  }
</style>

<div>
  <Router {url}>
    <div>
      <Route path="/" component={Home} />
      <Route path="/featured" component={Featured} />
      <Route path="/completed" component={Completed} />
      <Route path="/category/:category" let:params>
        <Category category={params.category} />
      </Route>
      <Route path="/novel/:name" let:params>
        <Novel name={params.name} />
      </Route>
      <Route path="/:name/:chapter" let:params>
        <Read name={params.name} chapter={params.chapter} />
      </Route>
    </div>
  </Router>
</div>
