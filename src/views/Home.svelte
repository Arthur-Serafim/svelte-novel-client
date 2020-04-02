<script>
  import axios from "axios";
  import { navigate } from "svelte-routing";
  import Navbar from "../components/Navbar.svelte";
  import Banner from "../components/Banner.svelte";
  import Scroller from "../components/Scroller.svelte";
  import ScrollerGhost from "../components/ScrollerGhost.svelte";

  async function getFeatured() {
    let response = await axios.get("http://localhost:3000/list/featured");
    return response.data;
  }

  async function getCompletedNovels() {
    let completed = await axios.post("http://localhost:3000/list/completed", {
      page: 1
    });

    return completed.data;
  }

  $: height = 0;
  let random = Math.floor(Math.random() * 10);
  let featured = getFeatured();
  let showSynopsis = false;
  let completedNovels = getCompletedNovels();
</script>

<style lang="scss">
  div {
    font-family: "Merriweather";
  }

  .hide-overflow {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .featured-container {
    height: 100%;
    background: #f5f6fc;
    box-sizing: border-box;
    padding: 25px 300px;
  }

  .section-header {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 25px;
  }

  .section-title {
    margin: 0;
    font-size: 36px;
  }

  .section-link {
    color: #1a4cff;
    cursor: pointer;
  }

  .section-link:hover {
    text-decoration: underline;
  }

  .weekly-container {
    display: grid;
    grid-template-columns: 250px 1fr;
    grid-template-rows: 1fr;
    grid-template-areas: "featured display";
    grid-gap: 50px;
  }

  .featured-novel {
    grid-area: featured;
    width: 100%;
    height: 100%;

    height: max-content;
    background-color: white;
    box-sizing: border-box;
    padding: 15px;
    overflow: hidden;
    transition: all 1s;
  }

  .featured-novel-loading {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 450px;
  }

  .featured-image {
    width: 100%;
    object-fit: cover;
    cursor: pointer;
  }

  .featured-image:hover {
    opacity: 0.75;
    transition: all 0.3s;
  }

  .featured-title {
    margin: 10px 0 5px 0;
    font-size: 16px;
    cursor: pointer;
  }

  .featured-title:hover {
    text-decoration: underline;
  }

  .featured-author {
    color: #252525;
    opacity: 70%;
    font-weight: bold;
    font-size: 12px;
    margin: 0;
    margin-bottom: 10px;
  }

  .featured-synopsis {
    white-space: pre-line;
    font-size: 12px;
  }

  .buttons {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 10px;
  }

  .read {
    border: none;
    box-shadow: none;

    background-color: #56d353;
    color: white;
    text-transform: uppercase;
    outline: none;
    height: 25px;
    width: 75px;
    border-radius: 25px;
    cursor: pointer;
    font-size: 12px;
    transition: all 0.3s;
  }

  .read:hover {
    opacity: 0.9;
  }

  .read-full {
    border: none;
    box-shadow: none;

    background-color: #56d353;
    color: white;
    text-transform: uppercase;
    outline: none;
    height: 30px;
    font-weight: bold;
    margin-top: 15px;
    margin-bottom: 0;
    width: 100%;
    cursor: pointer;
    font-size: 12px;

    transition: all 0.3s;
  }

  .read-full:hover {
    opacity: 0.7;
  }

  .show-more {
    font-size: 12px;
    color: #1a4cff;
    cursor: pointer;
  }

  .show-more:hover {
    text-decoration: underline;
    text-decoration-color: #1a4cff;
  }

  .listing-container {
    grid-area: display;
    width: 100%;
    height: 100%;
    overflow-y: auto;
    position: relative;

    display: flex;
    flex-wrap: wrap;

    scrollbar-width: none;
  }

  .listing-container::-webkit-scrollbar {
    display: none;
  }

  .list-image {
    width: 100%;
    height: 270px;
    object-fit: cover;
    transition: all 0.3s;
  }

  .list-image:hover {
    opacity: 0.9;
  }

  .list-container {
    width: 20%;
    background: white;
    box-sizing: border-box;
    padding: 10px;
    margin-bottom: 30px;
    margin-right: 25px;
  }

  .completed-container {
    box-sizing: border-box;
    padding: 0 300px;
    background: #f5f6fc;
    height: 100%;
    padding-bottom: 25px;
  }

  .category-container {
    box-sizing: border-box;
    padding: 0 300px;
    background: #f5f6fc;
    height: 100%;
    padding-bottom: 25px;
  }

  .loading {
    display: inline-block;
    position: relative;
    width: 80px;
    height: 80px;
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

<div class="container">
  <Navbar />
  <Banner />
  <section class="featured-container">
    <div class="section-header">
      <h2 class="section-title">Weekly Featured</h2>
      <span class="section-link">See More →</span>
    </div>
    {#await featured}
      <div class="weekly-container" bind:clientHeight={height}>
        <div class="featured-novel featured-novel-loading">
          <div class="loading">
            <div />
            <div />
          </div>
        </div>
        <div class="listing-container" style={`height: ${height}px`}>
          {#each Array(9) as _}
            <div
              class="list-container"
              style="width: 210px; height: 379px; display: flex; align-items:
              center; justify-content: center;">
              <div class="loading">
                <div />
                <div />
              </div>
            </div>
          {/each}
        </div>
      </div>
    {:then data}
      <div class="weekly-container">
        <div class="featured-novel" bind:clientHeight={height}>
          <img
            src={data.Xianxia[random].image}
            alt={`${data.Xianxia[random].title} Image`}
            class="hoverable-image featured-image" />
          <h2 class="featured-title hide-overflow">
            {data.Xianxia[random].title}
          </h2>
          <h2 class="featured-author hide-overflow">
            {data.Xianxia[random].author}
          </h2>
          <span class="featured-synopsis">
            {showSynopsis ? data.Xianxia[random].synopsis.trim() : data.Xianxia[
                  random
                ].synopsis
                  .substr(0, 175)
                  .trim() + '...'}
          </span>
          <div class="buttons">
            <button class="read">Read</button>
            <span
              class="show-more"
              on:click={() => (showSynopsis = !showSynopsis)}>
              {showSynopsis ? 'See less ↑' : 'See more ↓'}
            </span>
          </div>
        </div>
        <div class="listing-container" style={`height: ${height}px`}>
          {#each data.Xianxia as novel}
            {#if novel.link !== data.Xianxia[random].link}
              <div class="list-container">
                <img
                  class="list-image"
                  src={novel.image}
                  alt={`Image for novel ${novel.title}`} />
                <h2 class="featured-title hide-overflow">{novel.title}</h2>
                <h2 class="featured-author hide-overflow">{novel.author}</h2>
                <button class="read-full">See More</button>
              </div>
            {/if}
          {/each}
        </div>
      </div>
    {:catch error}
      error
    {/await}
  </section>
  <section class="completed-container">
    <div class="section-header">
      <h2 class="section-title">Completed Novels</h2>
      <span class="section-link">See More →</span>
    </div>
    {#await completedNovels}
      <ScrollerGhost name="home-ghost" />
    {:then completedNovels}
      <Scroller name="home" {completedNovels} />
    {/await}
  </section>
  <section class="category-container">
    <div class="section-header">
      <h2 class="section-title">Categories</h2>
    </div>
  </section>
</div>
