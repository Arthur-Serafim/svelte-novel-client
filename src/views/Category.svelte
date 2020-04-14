<script>
  import axios from "axios";
  import _ from "lodash";
  import Navbar from "../components/Navbar.svelte";
  import BASE_URL from "../BASE_URL.js";
  import { navigate } from "svelte-routing";
  export let category;
  let accumulator = [];
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
  let loading = false;
  let test = category;

  async function getCategory() {
    loaded = false;
    let response = await axios.post(`${BASE_URL}/list/category`, {
      category: category
        .split("-")
        .join("")
        .toLowerCase(),
      page: 1
    });
    accumulator = response.data;
    loaded = true;
    return response.data;
  }

  async function getMore(pageNumber) {
    loading = true;
    let response = await axios.post(`${BASE_URL}/list/category`, {
      category: category.toLowerCase(),
      page: pageNumber
    });
    accumulator = _.uniq([...accumulator, ...response.data], "link");
    loading = false;
    page += 1;
    return response.data;
  }

  $: if (category !== test) {
    getCategory();
  }

  function handleNavigate(item) {
    navigate(`/category/${item}`, { replace: true });
    getCategory();
  }

  let starter = getCategory();
</script>

<style>
  .category-container {
    width: 100%;
    height: 200px;
    background-color: #f2f2f2;
    box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding: 50px 300px;

    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    color: #252525;
  }

  @media only screen and (max-width: 1700px) {
    .category-container {
      padding: 0 250px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .category-container {
      padding: 0 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .category-container {
      padding: 0 150px;
    }
  }

  @media only screen and (max-width: 850px) {
    .category-container {
      padding: 0 100px;
    }
  }

  @media only screen and (max-width: 655px) {
    .category-container {
      padding: 0 50px;
    }
  }

  @media only screen and (max-width: 630px) {
    .category-container {
      height: 150px;
    }
  }

  @media only screen and (max-width: 450px) {
    .category-container {
      padding: 0 20px;
    }
  }

  .category-title {
    font-size: 20px;
  }

  .category-selector {
    display: flex;
    justify-content: space-between;
    font-weight: bold;
    flex-wrap: wrap;
  }

  .category-item {
    cursor: pointer;
    opacity: 0.5;
    margin-right: 5px;
    margin-bottom: 5px;
  }

  @media only screen and (max-width: 630px) {
    .category-item {
      font-size: 12px;
    }
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

  @media only screen and (max-width: 1700px) {
    .category-list {
      padding: 40px 250px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .category-list {
      padding: 30px 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .category-list {
      padding: 20px 150px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .category-list {
      padding: 20px 100px;
    }
  }

  @media only screen and (max-width: 885px) {
    .category-list {
      padding: 20px 50px;
    }
  }

  @media only screen and (max-width: 450px) {
    .category-list {
      padding: 20px 20px;
    }
  }

  .novel-card {
    width: 45%;
    display: flex;
    height: 230px;
    margin-bottom: 50px;
  }

  @media only screen and (max-width: 860px) {
    .novel-card {
      width: 100%;
    }
  }

  .novel-card-image {
    width: 150px;
    object-fit: cover;
    border-radius: 5px;
    box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.25);
    transition: all 0.3s;
  }

  @media only screen and (max-width: 1200px) {
    .novel-card-image {
      width: 125px;
    }

    .novel-card-image-container {
      width: 125px !important;
      margin-right: 25px;
    }

    .novel-card {
      height: 200px;
      margin-bottom: 30px;
    }
  }

  @media only screen and (max-width: 480px) {
    .novel-card-image {
      width: 100px;
    }

    .novel-card-image-container {
      width: 100px !important;
      height: 140px;
      margin-right: 15px;
    }

    .novel-card {
      margin-bottom: 20px;
      height: 140px;
    }
  }

  .novel-card-image-container {
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
    height: max-content;
    width: calc(100% - 180px);
    margin-left: 15px;
  }

  @media only screen and (max-width: 1200px) {
    .novel-card-info {
      margin: 0 5px;
      width: calc(100% - 160px);
    }
  }

  @media only screen and (max-width: 480px) {
    .novel-card-info {
      margin: 0 5px;
      width: calc(100% - 133px);
    }
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

  @media only screen and (max-width: 1200px) {
    .novel-card-title {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 450px) {
    .novel-card-title {
      font-size: 12px;
    }
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
    text-overflow: ellipsis;
  }

  @media only screen and (max-width: 1200px) {
    .novel-card-synopsis {
      font-size: 12px;
    }
  }

  @media only screen and (max-width: 480px) {
    .novel-card-synopsis {
      font-size: 10px;
    }
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
    height: calc(100vh - 240px);
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
      transform: scale(0.5);
      margin-left: -5px;
      transform: scale(0.6);
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

  .box {
    width: 135px;
    height: 200px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-radius: 15px;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    cursor: pointer;
    transition: all 0.3s;
    overflow: hidden;
    position: relative;
    background: #f2f2f2;
  }

  @media only screen and (max-width: 1700px) {
    .box {
      height: 180px;
      width: 121px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .box {
      height: 162px;
      width: 109px;
    }
  }

  @media only screen and (max-width: 1400px) {
    .box {
      height: 146px;
      width: 98px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .box {
      height: 131px;
      width: 88px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .box {
      height: 118px;
      width: 80px;
    }
  }

  @media only screen and (max-width: 750px) {
    .box {
      height: 106px;
      width: 72px;
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
                {novel.synopsis.substr(0, 150).trim() + '...'}
              </p>
            </div>
          </div>
        {/each}
        <div class="box" on:click={() => getMore(page + 1)}>
          {#if loading}
            <div class="loading">
              <div />
              <div />
            </div>
          {:else}
            <i class="fas fa-plus plus-icon" />
          {/if}
        </div>
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
