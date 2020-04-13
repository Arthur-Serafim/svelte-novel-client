<script>
  import axios from "axios";
  import { navigate } from "svelte-routing";
  import Navbar from "../components/Navbar.svelte";
  export let phrase;

  async function getData() {
    let response = await axios.post("http://localhost:3000/filter", {
      novel: phrase
    });
    return response.data;
  }

  let data = getData();
</script>

<style>
  .novels-container {
    box-sizing: border-box;
    padding: 15px 300px;
  }

  @media only screen and (max-width: 1700px) {
    .novels-container {
      padding: 15px 250px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .novels-container {
      padding: 15px 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .novels-container {
      padding: 15px 150px;
    }
  }

  @media only screen and (max-width: 850px) {
    .novels-container {
      padding: 15px 100px;
    }
  }

  @media only screen and (max-width: 655px) {
    .novels-container {
      padding: 15px 50px;
    }
  }

  @media only screen and (max-width: 450px) {
    .novels-container {
      padding: 15px 20px;
    }
  }

  .novel-item {
    display: flex;
  }

  .novel-title {
    font-weight: bold;
    cursor: pointer;
  }

  @media only screen and (max-width: 600px) {
    .novel-title {
      font-size: 12px;
    }
  }

  .novel-title:hover {
    text-decoration: underline;
  }

  .novel-influence {
    width: 30px;
    height: 30px;
    background: #56d353;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-size: 18px;
    border-radius: 5px;
    margin-right: 15px;
  }

  @media only screen and (max-width: 600px) {
    .novel-influence {
      width: 20px;
      height: 20px;
      font-size: 14px;
    }
  }

  .novel-info {
    display: flex;
    flex-direction: column;
  }

  .novel {
    margin: 0;
    margin-top: 5px;
    font-size: 14px;
    font-weight: bold;
    color: rgba(0, 0, 0, 0.4);
  }

  @media only screen and (max-width: 600px) {
    .novel {
      font-size: 12px;
    }
  }

  .spacer {
    background-color: rgba(0, 0, 0, 0.1);
    margin: 25px 0;
  }

  @media only screen and (max-width: 600px) {
    .spacer {
      margin: 10px 0;
    }
  }
</style>

<div>
  <Navbar />
  {#await data}
    loading
  {:then novels}
    <div class="novels-container">
      {#each novels as novel}
        <div class="novel-item">
          <div class="novel-influence">{novel.relevance}</div>
          <div
            class="novel-info"
            on:click={() => navigate(`/novel${novel.link}`)}>
            <span class="novel-title">{novel.title}</span>
            <div class="novel">{novel.author} - {novel.category}</div>
          </div>
        </div>
        <hr class="spacer" />
      {/each}
    </div>
  {/await}
</div>
