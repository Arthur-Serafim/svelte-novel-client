<script>
  export let category = "Xianxia";
  export let color = "#FE6B64";
  import { navigate } from "svelte-routing";
</script>

<style>
  .category-card-widget {
    height: 150px;
    width: 25%;
    box-sizing: border-box;
    padding: 15px;
    border-radius: 15px;
    color: white;
    overflow: hidden;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    cursor: pointer;
    margin-bottom: 25px;
    margin-right: 30px;
  }

  @media only screen and (max-width: 1000px) {
    .category-card-widget {
      width: 40%;
      height: 125px;
    }
  }

  .category-card-widget:hover {
    opacity: 0.8;
  }

  .category-card-title {
    font-weight: bold;
    font-size: 18px;
  }

  @media only screen and (max-width: 450px) {
    .category-card-title {
      font-size: 14px;
    }
  }
</style>

<div
  class="category-card-widget"
  style={`background: ${color}`}
  on:click={() => navigate(`/category/${category}`)}>
  <span class="category-card-title">{category}</span>
</div>
