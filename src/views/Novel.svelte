<script>
  import axios from "axios";
  import Navbar from "../components/Navbar.svelte";
  import { navigate } from "svelte-routing";
  export let name;
  let navbar;

  async function getNovel(path) {
    path = "/" + path + "/";
    try {
      let response = await axios.post("http://localhost:3000/get/novel", {
        link: path
      });
      return response.data;
    } catch (error) {
      console.log(error);
      return { name: error.message };
    }
  }

  async function getChapter(path) {
    path = "/" + path + "/";
    try {
      let response = await axios.post("http://localhost:3000/list/chapters", {
        link: path
      });
      return response.data;
    } catch (error) {
      console.log(error);
      return { name: error.message };
    }
  }

  function handleNavigate(link) {
    link = String(link)
      .split(".html")
      .join("");
    navigate(`/${name}/${link}`);
  }

  let novel = getNovel(name);
  let chapters = getChapter(name);
</script>

<style>
  .novel-showcase {
    height: 375px;
    width: 100%;
    box-sizing: border-box;
    padding: 0 300px;
    background: #f5f6fc;

    box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.25);
    display: flex;
    z-index: 10;
  }

  @media only screen and (max-width: 1700px) {
    .novel-showcase {
      padding: 0 250px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .novel-showcase {
      padding: 0 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .novel-showcase {
      padding: 0 150px;
      height: 300px;
    }
  }

  @media only screen and (max-width: 850px) {
    .novel-showcase {
      padding: 0 100px;
      height: 235px;
    }
  }

  @media only screen and (max-width: 655px) {
    .novel-showcase {
      padding: 0 50px;
    }
  }

  @media only screen and (max-width: 560px) {
    .novel-showcase {
      height: 170px;
    }
  }

  @media only screen and (max-width: 450px) {
    .novel-showcase {
      padding: 0 20px;
    }
  }

  .novel-image {
    margin-top: 75px;
    height: 300px;
    margin-right: 30px;
    object-fit: cover;
  }

  @media only screen and (max-width: 1100px) {
    .novel-image {
      height: 250px;
      margin-top: 50px;
    }
  }

  @media only screen and (max-width: 850px) {
    .novel-image {
      height: 200px;
      margin-top: 35px;
    }
  }

  @media only screen and (max-width: 560px) {
    .novel-image {
      height: 150px;
      margin-top: 20px;
    }
  }

  .novel-info {
    overflow: hidden;
    width: 100%;
    margin-top: 75px;
    height: 80%;
  }

  @media only screen and (max-width: 1100px) {
    .novel-info {
      margin-top: 50px;
    }
  }

  @media only screen and (max-width: 850px) {
    .novel-info {
      margin-top: 35px;
    }
  }

  @media only screen and (max-width: 560px) {
    .novel-info {
      margin-top: 20px;
    }
  }

  .novel-title {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin: 0;
    margin-bottom: 5px;
  }

  @media only screen and (max-width: 1400px) {
    .novel-title {
      font-size: 20px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .novel-title {
      font-size: 16px;
    }
  }

  @media only screen and (max-width: 700px) {
    .novel-title {
      font-size: 14px;
    }
  }

  .novel-details {
    width: 100%;
    opacity: 50%;
    display: flex;
    flex-direction: column;
  }

  .novel-details-item {
    display: flex;
    align-items: center;
    margin-right: 30px;
    margin-bottom: 5px;
  }

  .novel-details-item:last-child {
    margin-bottom: 0;
  }

  @media only screen and (max-width: 1400px) {
    .novel-details-item {
      margin-right: 15px;
    }
  }

  .novel-icon {
    font-size: 16px;
    margin-right: 5px;
  }

  @media only screen and (max-width: 1400px) {
    .novel-icon {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 700px) {
    .novel-icon {
      font-size: 12px;
    }
  }

  @media only screen and (max-width: 560px) {
    .novel-icon {
      font-size: 10px;
      margin: 0 5px 0 0;
    }
  }

  .novel-label {
    font-size: 14px;
    font-weight: bold;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  @media only screen and (max-width: 1400px) {
    .novel-label {
      font-size: 12px;
    }
  }

  @media only screen and (max-width: 700px) {
    .novel-label {
      font-size: 10px;
    }
  }

  @media only screen and (max-width: 560px) {
    .novel-label {
      font-size: 8px;
    }
  }

  .novel-synopsis {
    color: #252525;
    opacity: 0.75;
    margin-top: 15px;
    line-height: 1.2;
    white-space: pre-line;

    height: 160px;
    overflow-y: auto;

    padding-right: 25px;
    z-index: 1000;
  }

  @media only screen and (max-width: 1100px) {
    .novel-synopsis {
      height: 130px;
      font-size: 14px;
      margin-top: 10px;
    }
  }

  @media only screen and (max-width: 850px) {
    .novel-synopsis {
      height: 90px;
      font-size: 12px;
      margin-top: 5px;
    }
  }

  @media only screen and (max-width: 700px) {
    .novel-synopsis {
      font-size: 10px;
    }
  }

  @media only screen and (max-width: 560px) {
    .novel-synopsis {
      font-size: 8px;
      height: 65px;
    }
  }

  .chapter-content {
    box-sizing: border-box;
    padding: 35px 300px;
    min-height: 500px;
    z-index: -5;
  }

  @media only screen and (max-width: 1700px) {
    .chapter-content {
      padding: 25px 250px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .chapter-content {
      padding: 20px 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .chapter-content {
      padding: 15px 150px;
    }
  }

  @media only screen and (max-width: 850px) {
    .chapter-content {
      padding: 10px 100px;
    }
  }

  @media only screen and (max-width: 655px) {
    .chapter-content {
      padding: 10px 50px;
    }
  }

  @media only screen and (max-width: 450px) {
    .chapter-content {
      padding: 10px 20px;
    }
  }

  .chapter-section {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .chapter-section-title {
    font-size: 32px;
    font-weight: bold;
    color: #252525;
    text-decoration: underline;
  }

  @media only screen and (max-width: 1400px) {
    .chapter-section-title {
      font-size: 28px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .chapter-section-title {
      font-size: 26px;
    }
  }

  @media only screen and (max-width: 900px) {
    .chapter-section-title {
      font-size: 24px;
    }
  }

  @media only screen and (max-width: 700px) {
    .chapter-section-title {
      font-size: 22px;
    }
  }

  .chapter-listage {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin-top: 30px;
  }

  .chapter-item {
    width: 45%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin: 15px 0;
    margin-left: 5px;
  }

  @media only screen and (max-width: 900px) {
    .chapter-item {
      margin: 10px 0;
    }
  }

  .line {
    color: #252525;
    opacity: 0.3;
  }

  .chapter-name {
    cursor: pointer;
  }

  @media only screen and (max-width: 1400px) {
    .chapter-name {
      font-size: 16px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .chapter-name {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 900px) {
    .chapter-name {
      font-size: 12px;
    }
  }

  @media only screen and (max-width: 700px) {
    .chapter-name {
      font-size: 10px;
    }
  }

  .chapter-name:hover {
    text-decoration: underline;
    color: #1a4cff;
  }

  .novel-loading {
    background-color: #f5f6fc;
    height: calc(100vh - 60px);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .bookmark-button {
    background: #fe6b64;
    color: white;
    border: none;
    border-radius: 5px;

    font-size: 16px;
    cursor: pointer;
    margin: 0;
  }

  @media only screen and (max-width: 1100px) {
    .bookmark-button {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 900px) {
    .bookmark-button {
      font-size: 12px;
    }
  }

  @media only screen and (max-width: 700px) {
    .bookmark-button {
      font-size: 10px;
    }
  }

  .bookmark-button:hover {
    opacity: 0.8;
  }

  .hide-overflow {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
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
  <Navbar bind:this={navbar} />
  {#await novel}
    <div class="novel-loading">
      <div class="loading">
        <div />
        <div />
      </div>
    </div>
  {:then novel}
    <div class="novel-body">
      <div class="novel-showcase">
        <img
          src={novel.image}
          alt={`Image for ${novel.title}`}
          class="novel-image" />
        <div class="novel-info">
          <h2 class="novel-title hide-overflow">{novel.title}</h2>
          <div class="novel-details">
            <div class="novel-details-item">
              <i class="novel-icon fas fa-book-open" />
              <span class="novel-label hide-overflow">{novel.category}</span>
            </div>
            <div class="novel-details-item">
              <i class="novel-icon fas fa-edit" />
              <span class="novel-label hide-overflow">{novel.update}</span>
            </div>
            <div class="novel-details-item">
              <i class="novel-icon fas fa-user" />
              <span class="novel-label hide-overflow">{novel.author}</span>
            </div>
          </div>
          <div class="novel-synopsis">{novel.synopsis.trim()}</div>
        </div>

      </div>
      <div class="chapter-content">
        <div class="chapter-section">
          <span class="chapter-section-title">Content</span>
          <button class="bookmark-button">Bookmark</button>
        </div>
        {#await chapters}
          loading
        {:then chapters}
          <div class="chapter-listage">
            {#each chapters as item}
              <div class="chapter-item">
                <div
                  class="chapter-name hide-overflow"
                  on:click={() => handleNavigate(item.link)}>
                  {item.title}
                </div>
                <hr class="line" />
              </div>
            {/each}
          </div>
        {/await}
      </div>
    </div>
  {/await}
</div>
