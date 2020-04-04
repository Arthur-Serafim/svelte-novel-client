<script>
  import axios from "axios";
  import Navbar from "../components/Navbar.svelte";
  import { navigate } from "svelte-routing";
  export let name;

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
    link = link.slipt(".html").join("");
    navigate(`/novel${novel.link}${link}`);
  }

  let novel = getNovel(name);
  let chapters = getChapter(name);
</script>

<style>
  .novel-body {
  }

  .novel-showcase {
    height: min-content;
    width: 100%;
    box-sizing: border-box;
    padding: 0 300px;
    background: #f5f6fc;

    box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.25);
    display: flex;
  }

  .novel-image {
    margin-top: 75px;
    height: 300px;
    margin-right: 30px;
    object-fit: cover;
  }

  .novel-info {
    overflow: hidden;
    width: 100%;
    margin-top: 75px;
    height: 80%;
  }

  .novel-title {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin: 0;
    margin-bottom: 5px;
  }

  .novel-details {
    width: 100%;
    opacity: 50%;
    display: flex;
  }

  .novel-details-item {
    display: flex;
    align-items: center;
    margin-right: 30px;
  }

  .novel-icon {
    font-size: 16px;
    margin-right: 5px;
  }

  .novel-label {
    font-size: 14px;
    font-weight: bold;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .novel-synopsis {
    color: #252525;
    opacity: 0.75;
    margin-top: 15px;
    line-height: 1.2;
    white-space: pre-line;
  }

  .chapter-content {
    background: white;
    box-sizing: border-box;
    padding: 35px 300px;
    min-height: 500px;
  }

  .chapter-section {
    font-size: 32px;
    font-weight: bold;
    color: #252525;
    text-decoration: underline;
  }

  .chapter-listage {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin-top: 50px;
  }

  .chapter-item {
    width: 45%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin: 15px 0;
    margin-left: 5px;
  }

  .line {
    color: #252525;
    opacity: 0.3;
  }

  .chapter-name {
    cursor: pointer;
  }

  .chapter-name:hover {
    text-decoration: underline;
    color: #1a4cff;
  }
</style>

<div>
  <Navbar />
  {#await novel}
    loading...
  {:then novel}
    <div class="novel-body">
      <div class="novel-showcase">
        <img
          src={novel.image}
          alt={`Image for ${novel.title}`}
          class="novel-image" />
        <div class="novel-info">
          <h2 class="novel-title">{novel.title}</h2>
          <div class="novel-details">
            <div class="novel-details-item">
              <i class="novel-icon fas fa-book-open" />
              <span class="novel-label">{novel.category}</span>
            </div>
            <div class="novel-details-item">
              <i class="novel-icon fas fa-book-edit" />
              <span class="novel-label">{novel.update}</span>
            </div>
            <div class="novel-details-item">
              <i class="novel-icon fas fa-book-user" />
              <span class="novel-label">{novel.author}</span>
            </div>
          </div>
          <div class="novel-synopsis">{novel.synopsis}</div>
        </div>

      </div>
      <div class="chapter-content">
        <div class="chapter-section">Content</div>
        {#await chapters}
          loading
        {:then chapters}
          <div class="chapter-listage">
            {#each chapters as item}
              <div class="chapter-item">
                <div
                  class="chapter-name"
                  on:click={() => handleNavigate(item.link)}>
                  {item.title}
                </div>
                <div class="line" />
              </div>
            {/each}
          </div>
        {/await}
      </div>
    </div>
  {/await}
</div>
