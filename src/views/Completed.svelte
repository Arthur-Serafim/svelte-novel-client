<script>
  import _ from "lodash";
  import axios from "axios";
  import Navbar from "../components/Navbar.svelte";
  import BASE_URL from "../BASE_URL.js";
  import { fade } from "svelte/transition";
  import { navigate } from "svelte-routing";

  let page = 1;
  let accumulator = [];
  let loading = false;
  let show = false;

  async function getCompletedNovels(page) {
    let completed = await axios.post(`${BASE_URL}/list/completed`, {
      page: page
    });
    accumulator = [...accumulator, ...completed.data];
    accumulator = _.uniq(accumulator, "link");
    return completed.data;
  }

  async function handleLoad() {
    loading = true;
    let data = await getCompletedNovels(page + 1);
    page += 1;
    accumulator = [...accumulator, ...data];
    accumulator = _.uniq(accumulator, "link");

    loading = false;
  }

  let completedNovels = getCompletedNovels(page);
</script>

<style>
  .grid-container {
    height: 100%;
    min-height: 100vh;
    width: 100%;
    background-color: #f2f2f2;
    box-sizing: border-box;
    padding: 25px 0;
    display: grid;
    justify-content: center;
    grid-template-columns: repeat(7, min-content);
    grid-template-rows: repeat(10, min-content);
    grid-gap: 25px;
  }

  @media only screen and (max-width: 730px) {
    .grid-container {
      grid-template-columns: repeat(5, min-content);
    }
  }

  @media only screen and (max-width: 550px) {
    .grid-container {
      grid-template-columns: repeat(4, min-content);
    }
  }

  @media only screen and (max-width: 400px) {
    .grid-container {
      grid-template-columns: repeat(3, min-content);
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

  .white {
    background-color: white;
  }

  .default {
    cursor: default;
  }

  .box:hover .plus-icon {
    color: #1a4cff;
  }

  .plus-icon {
    font-size: 36px;
  }

  .box-image {
    height: 100%;
    width: 100%;
    object-fit: cover;
  }

  .box:hover .title-container {
    z-index: 10;
  }

  .title {
    margin: 10px 0 5px 0;
    font-size: 16px;
    cursor: pointer;
    text-align: center;
  }

  .title-container {
    background-color: white;
    height: 100%;
    width: 100%;
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    box-sizing: border-box;
    padding: 15px;
    z-index: -10;
    transition: all 0.3s;
  }

  @media only screen and (max-width: 1700px) {
    .title {
      font-size: 13px;
    }
  }

  @media only screen and (max-width: 1400px) {
    .title {
      font-size: 11px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .title {
      font-size: 9px;
    }
  }

  @media only screen and (max-width: 800px) {
    .title {
      font-size: 7px;
    }
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
  <div class="grid-container">
    {#await completedNovels}
      {#each Array(11) as item}
        <div class="box white default">
          <div class="loading">
            <div />
            <div />
          </div>
        </div>
      {/each}
    {:then data}
      {#each accumulator as item}
        <div
          class="box"
          on:click={() => navigate(`/novel${item.link}`)}
          on:mouseenter={() => (item.show = true)}
          on:mouseleave={() => (item.show = false)}>
          <img
            src={item.image}
            alt={`Image for ${item.title}`}
            class="box-image" />
          {#if item.show}
            <div class="title-container" transition:fade>
              <h2 class="title">{item.title}</h2>
            </div>
          {/if}
        </div>
      {/each}
      <div class="box white" on:click={handleLoad}>
        {#if loading}
          <div class="loading">
            <div />
            <div />
          </div>
        {:else}
          <i class="fas fa-plus plus-icon" />
        {/if}
      </div>
    {/await}
  </div>
</div>
