<script>
  import axios from "axios";
  import Navbar from "../components/Navbar.svelte";
  import { navigate } from "svelte-routing";
  export let category;
  let categories = [
    "Xianxia",
    "Romantic",
    "Historical",
    "Sci-fi",
    "Game",
    "Fantasy"
  ];
  let page = 1;
  let loaded = false;

  async function getCategory() {
    loaded = false;
    let response = await axios.post("http://localhost:3000/list/category", {
      category: category.toLowerCase(),
      page: page
    });
    accumulator = response.data;
    loaded = true;
    return response.data;
  }

  $: accumulator = [];

  function handleNavigate(item) {
    navigate(`/category/${item}`, { replace: true });
    getCategory();
  }

  let starter = getCategory();
</script>

<style>
  .category-container {
    width: 100%;
    height: 300px;
    background-color: #f2f2f2;
    box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding: 50px 300px;

    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    color: #252525;
  }

  .category-selector {
    display: flex;
    justify-content: space-between;
    font-weight: bold;
  }

  .category-item {
    cursor: pointer;
    opacity: 0.5;
  }

  .category-item:hover {
    color: #1a4cff;
    text-decoration: underline;
    text-decoration-color: #1a4cff;
    opacity: 1;
  }

  .category-item.active-category {
    color: #1a4cff;
    text-decoration: underline;
    text-decoration-color: #1a4cff;
    opacity: 1;
  }

  .category-list {
    box-sizing: border-box;
    padding: 50px 300px;
    display: flex;
    flex-wrap: wrap;
  }

  .novel-card {
    width: 50%;
    display: flex;
    height: 230px;
    margin-bottom: 50px;
  }

  .novel-card-image {
    height: 100%;
    width: 150px;
    border-radius: 5px;
    box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.25);
    transition: all 0.3s;
  }

  .novel-card-image-container {
    height: 100%;
    width: 150px;
    border-radius: 5px;
    overflow: hidden;
    transition: all 0.3s;
    cursor: pointer;
  }

  .novel-card-image-container:hover .novel-card-image {
    transform: scale(1.1);
    opacity: 0.8;
  }

  .novel-card-info {
    width: calc(100% - 180px);
    margin: 0 15px;
  }

  .novel-card-title {
    color: #252525;
    font-weight: bold;
    font-size: 16px;
    margin: 0;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    cursor: pointer;
  }

  .novel-card-title:hover {
    text-decoration: underline;
  }

  .novel-card-author {
    color: #252525;
    opacity: 0.7;
    font-weight: bold;
    font-size: 14px;
    margin: 5px 0;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .novel-card-synopsis {
    margin-top: 15px;
    color: #252525;
    white-space: pre-line;
    font-size: 14px;
    overflow: hidden;
  }

  .load-more {
    text-align: center;
    cursor: pointer;
    color: #1a4cff;
    width: 100%;
  }

  .load-more:hover {
    text-decoration: underline;
    text-decoration-color: #1a4cff;
  }

  .loading-container {
    height: 100px;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .loading {
    display: inline-block;
    position: relative;
    width: 80px;
    height: 80px;
  }

  @media only screen and (max-width: 1000px) {
    .loading {
      transform: scale(0.8);
    }
  }

  @media only screen and (max-width: 800px) {
    .loading {
      transform: scale(0.6);
    }
  }

  @media only screen and (max-width: 800px) {
    .loading {
      transform: scale(0.5);
      margin-left: -5px;
    }
  }

  .loading div {
    position: absolute;
    border: 4px solid #252525;
    opacity: 1;
    border-radius: 50%;
    animation: loading 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;
  }

  .loading div:nth-child(2) {
    animation-delay: -0.5s;
  }

  @keyframes loading {
    0% {
      top: 36px;
      left: 36px;
      width: 0;
      height: 0;
      opacity: 1;
    }
    100% {
      top: 0px;
      left: 0px;
      width: 72px;
      height: 72px;
      opacity: 0;
    }
  }
</style>

<div>
  <Navbar />
  <div class="category-container">
    <h2 class="category-title">Novel Genre</h2>
    <div class="category-selector">
      {#each categories as item}
        <div
          class={`category-item ${category === item && 'active-category'}`}
          on:click={() => handleNavigate(item)}>
          {item}
        </div>
      {/each}
    </div>
  </div>
  {#await starter}
    <div class="loading-container">
      <div class="loading">
        <div />
        <div />
      </div>
    </div>
  {:then data}
    <div class="category-list">
      {#if loaded}
        {#each accumulator as novel}
          <div class="novel-card">
            <div class="novel-card-image-container">
              <img
                src={novel.image}
                alt={`Image for ${novel.name}`}
                class="novel-card-image"
                on:click={() => navigate(`/novel${novel.link}`)} />
            </div>
            <div class="novel-card-info">
              <h2
                class="novel-card-title"
                on:click={() => navigate(`/novel${novel.link}`)}>
                {novel.title}
              </h2>
              <p class="novel-card-synopsis">
                {novel.synopsis.substr(0, 200).trim() + '...'}
              </p>
            </div>
          </div>
        {/each}
      {:else}
        <div class="loading-container">
          <div class="loading">
            <div />
            <div />
          </div>
        </div>
      {/if}
    </div>
  {/await}
</div>
