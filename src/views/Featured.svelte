<script>
  import axios from "axios";
  import BASE_URL from "../BASE_URL.js";
  import Navbar from "../components/Navbar.svelte";
  import Scroller from "../components/Scroller.svelte";
  import ScrollerGhost from "../components/ScrollerGhost.svelte";

  async function getFeatured() {
    let response = await axios.get(`${BASE_URL}/list/featured`);
    console.log(response.data);
    return response.data;
  }

  let category = [
    "Xianxia",
    "Romantic",
    "Historical",
    "Sci-fi",
    "Game",
    "Fantasy"
  ];

  let data = getFeatured();
</script>

<style>
  .featured-container {
    height: 100%;
    background: #f2f2f2;
    box-sizing: border-box;
    padding: 25px 300px;
  }

  @media only screen and (max-width: 1700px) {
    .featured-container {
      padding: 25px 250px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .featured-container {
      padding: 25px 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .featured-container {
      padding: 20px 150px;
    }
  }

  @media only screen and (max-width: 850px) {
    .featured-container {
      padding: 15px 100px;
    }
  }

  @media only screen and (max-width: 655px) {
    .featured-container {
      padding: 10px 50px;
    }
  }

  @media only screen and (max-width: 450px) {
    .featured-container {
      padding: 10px 20px;
    }
  }

  .category-title {
    margin: 15px 0;
    font-size: 32px;
    align-self: center;
  }

  @media only screen and (max-width: 1700px) {
    .category-title {
      font-size: 28px;
    }
  }

  @media only screen and (max-width: 1400px) {
    .category-title {
      font-size: 24px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .category-title {
      font-size: 22px;
    }
  }

  @media only screen and (max-width: 850px) {
    .category-title {
      font-size: 20px;
    }
  }
</style>

<div>
  <Navbar />
  <div class="featured-container">
    {#await data}
      {#each category as item}
        <div class="category-title">{item}</div>
        <ScrollerGhost />
      {/each}
    {:then data}
      {#each category as item}
        <div class="category-title">{item}</div>
        <Scroller completedNovels={data[item]} />
      {/each}
    {/await}
  </div>
</div>
