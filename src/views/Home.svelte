<script>
  import axios from "axios";
  import { navigate } from "svelte-routing";
  import Navbar from "../components/Navbar.svelte";
  import Banner from "../components/Banner.svelte";
  import Scroller from "../components/Scroller.svelte";
  import ScrollerGhost from "../components/ScrollerGhost.svelte";
  import CategoryCard from "../components/CategoryCard.svelte";
  import Footer from "../components/Footer.svelte";
  import BASE_URL from "../BASE_URL.js";

  async function getFeatured() {
    let response = await axios.get(`${BASE_URL}/list/featured`);
    return response.data.All;
  }

  async function getCompletedNovels() {
    let completed = await axios.post(`${BASE_URL}/list/completed`, {
      page: 1
    });

    return completed.data;
  }

  function handleNavigate(link) {
    navigate(`/novel${link}`, { replace: false });
  }

  $: height = 0;
  let random = Math.floor(Math.random() * 10);
  let featured = getFeatured();
  let showSynopsis = false;
  let completedNovels = getCompletedNovels();
  let category = [
    { name: "Xianxia", color: "#B29DD9" },
    { name: "Romantic", color: "#FE6B64" },
    { name: "Historical", color: "#FFB347" },
    { name: "Sci-Fi", color: "#77DD77" },
    { name: "Game", color: "#779ECB" },
    { name: "Fantasy", color: "#a8626a" }
  ];
</script>

<style lang="scss">
  div {
    font-family: "Merriweather";
  }

  .container {
    background: #f2f2f2;
  }

  .hide-overflow {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .featured-container {
    height: 100%;
    background: #f2f2f2;
    box-sizing: border-box;
    padding: 0 300px;
  }

  @media only screen and (max-width: 1700px) {
    .featured-container {
      padding: 0 250px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .featured-container {
      padding: 0 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .featured-container {
      padding: 0 150px;
    }
  }

  @media only screen and (max-width: 850px) {
    .featured-container {
      padding: 0 100px;
    }
  }

  @media only screen and (max-width: 655px) {
    .featured-container {
      padding: 0 50px;
    }
  }

  @media only screen and (max-width: 450px) {
    .featured-container {
      padding: 0 20px;
    }
  }

  .section-header {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 25px 0;
  }

  .section-title {
    margin: 0;
    font-size: 32px;
    align-self: center;
  }

  @media only screen and (max-width: 1700px) {
    .section-title {
      font-size: 28px;
    }
  }

  @media only screen and (max-width: 1400px) {
    .section-title {
      font-size: 24px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .section-title {
      font-size: 22px;
    }
  }

  @media only screen and (max-width: 850px) {
    .section-title {
      font-size: 20px;
    }
  }

  .section-link {
    color: #1a4cff;
    cursor: pointer;
  }

  @media only screen and (max-width: 1400px) {
    .section-link {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .section-link {
      font-size: 12px;
    }
  }

  @media only screen and (max-width: 850px) {
    .section-link {
      font-size: 10px;
    }
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

  @media only screen and (max-width: 1700px) {
    .weekly-container {
      grid-template-columns: 200px 1fr;
    }
  }

  @media only screen and (max-width: 1400px) {
    .weekly-container {
      grid-template-columns: 150px 1fr;
    }
  }

  @media only screen and (max-width: 1000px) {
    .weekly-container {
      grid-template-columns: 125px 1fr;
      grid-gap: 40px;
    }
  }

  @media only screen and (max-width: 850px) {
    .weekly-container {
      grid-template-columns: 125px 1fr;
      grid-gap: 30px;
    }
  }

  @media only screen and (max-width: 655px) {
    .weekly-container {
      grid-template-columns: 150px 1fr;
      grid-gap: 30px;
    }
  }

  @media only screen and (max-width: 480px) {
    .weekly-container {
      grid-template-columns: 110px 1fr;
      grid-gap: 25px;
    }
  }

  @media only screen and (max-width: 440px) {
    .weekly-container {
      grid-template-columns: 150px 1fr;
      grid-gap: 25px;
    }
  }

  @media only screen and (max-width: 400px) {
    .weekly-container {
      grid-template-columns: 130px 1fr;
      grid-gap: 25px;
    }
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

  @media only screen and (max-width: 1500px) {
    .featured-novel {
      padding: 5px;
    }
  }

  .featured-novel-loading {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 450px;
  }

  @media only screen and (max-width: 1400px) {
    .featured-novel-loading {
      height: 350px;
    }
  }

  @media only screen and (max-width: 480px) {
    .featured-novel-loading {
      height: 250px;
    }
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

  @media only screen and (max-width: 1700px) {
    .featured-title {
      font-size: 13px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .featured-title {
      font-size: 11px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .featured-title {
      font-size: 9px;
    }
  }

  @media only screen and (max-width: 800px) {
    .featured-title {
      font-size: 7px;
    }
  }

  @media only screen and (max-width: 440px) {
    .featured-title {
      font-size: 9px;
    }
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

  @media only screen and (max-width: 1700px) {
    .featured-author {
      font-size: 10px;
      margin-bottom: 5px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .featured-author {
      font-size: 8px;
      margin-bottom: 5px;
    }
  }

  .featured-synopsis {
    white-space: pre-line;
    font-size: 12px;
  }

  @media only screen and (max-width: 1400px) {
    .featured-synopsis {
      font-size: 8px;
    }
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

  @media only screen and (max-width: 1400px) {
    .read {
      width: 60px;
      height: 20px;
      font-size: 8px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .read {
      width: 50px;
      height: 20px;
      font-size: 8px;
      margin-bottom: -2px;
    }
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
    align-self: flex-end;
    margin-bottom: 0;
    width: 100%;
    cursor: pointer;
    font-size: 12px;

    transition: all 0.3s;
  }

  @media only screen and (max-width: 1500px) {
    .read-full {
      font-size: 10px;
      height: 25px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .read-full {
      font-size: 8px;
      margin-top: 5px;
      height: 20px;
    }
  }

  .read-full:hover {
    opacity: 0.7;
  }

  .show-more {
    font-size: 12px;
    color: #1a4cff;
    cursor: pointer;
  }

  @media only screen and (max-width: 1400px) {
    .show-more {
      font-size: 10px;
    }
  }

  @media only screen and (max-width: 850px) {
    .show-more {
      font-size: 8px;
    }
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

  @media only screen and (max-width: 440px) {
    .listing-container {
      justify-content: center;
    }
  }

  .listing-container::-webkit-scrollbar {
    display: none;
  }

  .list-image {
    width: 100%;
    object-fit: cover;
    transition: all 0.3s;
    cursor: pointer;
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

  @media only screen and (max-width: 1500px) {
    .list-container {
      padding: 5px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .list-container {
      margin-right: 15px;
    }
  }

  @media only screen and (max-width: 850px) {
    .list-container {
      margin-right: 10px;
      width: 30%;
    }
  }

  @media only screen and (max-width: 655px) {
    .list-container {
      width: 45%;
    }
  }

  @media only screen and (max-width: 440px) {
    .list-container {
      width: 80%;
    }
  }

  .list-container-align {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  .list-container-loading {
    width: 210px;
    height: 379px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  @media only screen and (max-width: 1700px) {
    .list-container-loading {
      width: 170px;
      height: 331px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .list-container-loading {
      width: 150px;
      height: 300px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .list-container-loading {
      width: 140px;
      height: 280px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .list-container-loading {
      width: 105px;
      height: 226px;
    }
  }

  @media only screen and (max-width: 850px) {
    .list-container-loading {
      width: 138px;
      height: 254px;
    }
  }

  @media only screen and (max-width: 580px) {
    .list-container-loading {
      width: 45%;
      height: 240px;
    }
  }

  @media only screen and (max-width: 440px) {
    .list-container-loading {
      width: 80%;
      height: 200px;
    }
  }

  .completed-container {
    box-sizing: border-box;
    padding: 0 300px;
    background: #f2f2f2;
    height: 100%;
    padding-bottom: 25px;
  }

  @media only screen and (max-width: 1700px) {
    .completed-container {
      padding: 0 250px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .completed-container {
      padding: 0 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .completed-container {
      padding: 0 150px;
    }
  }

  @media only screen and (max-width: 850px) {
    .completed-container {
      padding: 0 100px;
    }
  }

  @media only screen and (max-width: 655px) {
    .completed-container {
      padding: 0 50px;
    }
  }

  @media only screen and (max-width: 450px) {
    .completed-container {
      padding: 0 20px;
    }
  }

  .category-container {
    box-sizing: border-box;
    padding: 0 300px;
    background: #f2f2f2;
    height: 100%;
    padding-bottom: 25px;
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

  @media only screen and (max-width: 450px) {
    .category-container {
      padding: 0 20px;
    }
  }

  .category-card-container {
    display: flex;
    flex-wrap: wrap;
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
            <div class="list-container list-container-loading">
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
            src={data[random].image}
            alt={`${data[random].title} Image`}
            class="hoverable-image featured-image"
            on:click={() => handleNavigate(data[random].link)} />
          <h2
            class="featured-title hide-overflow"
            on:click={() => handleNavigate(data[random].link)}>
            {data[random].title}
          </h2>
          <h2 class="featured-author hide-overflow">{data[random].author}</h2>
          <span class="featured-synopsis">
            {showSynopsis ? data[random].synopsis.trim() : data[random].synopsis
                  .substr(0, 175)
                  .trim() + '...'}
          </span>
          <div class="buttons">
            <button
              class="read"
              on:click={() => handleNavigate(data[random].link)}>
              Read
            </button>
            <span
              class="show-more"
              on:click={() => (showSynopsis = !showSynopsis)}>
              {showSynopsis ? 'See less ↑' : 'See more ↓'}
            </span>
          </div>
        </div>
        <div class="listing-container" style={`height: ${height}px`}>
          {#each data as novel}
            {#if novel.link !== data[random].link}
              <div class="list-container list-container-align">
                <div>
                  <img
                    class="list-image"
                    src={novel.image}
                    alt={`Image for novel ${novel.title}`}
                    on:click={() => handleNavigate(novel.link)} />
                  <h2
                    class="featured-title hide-overflow"
                    on:click={() => handleNavigate(novel.link)}>
                    {novel.title}
                  </h2>
                  <h2 class="featured-author hide-overflow">{novel.author}</h2>
                </div>
                <button
                  class="read-full"
                  on:click={() => handleNavigate(novel.link)}>
                  See More
                </button>
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
      <ScrollerGhost />
    {:then completedNovels}
      <Scroller {completedNovels} />
    {/await}
  </section>
  <section class="category-container">
    <div class="section-header">
      <h2 class="section-title">Discover Novels</h2>
    </div>
    <div class="category-card-container">
      {#each category as item}
        <CategoryCard category={item.name} color={item.color} />
      {/each}
    </div>
  </section>
  <Footer />
</div>
