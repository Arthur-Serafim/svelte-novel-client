<script>
  let inputNovel = "";

  import { navigate } from "svelte-routing";

  function handleSubmit() {
    navigate(`/search/${inputNovel}`, { replace: true });
  }
</script>

<style lang="scss">
  .navigation {
    background: white;
    color: #252525;
    opacity: 0.7;
    width: 100%;
    height: 60px;
    box-sizing: border-box;
    padding: 0 300px;

    display: flex;
    align-items: center;
    font-family: "Merriweather";
  }

  @media only screen and (max-width: 1700px) {
    .navigation {
      padding: 0 250px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .navigation {
      height: 45px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .navigation {
      padding: 0 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .navigation {
      padding: 0 150px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .navigation {
      height: 40px;
    }
  }

  @media only screen and (max-width: 850px) {
    .navigation {
      padding: 0 100px;
    }
  }

  @media only screen and (max-width: 655px) {
    .navigation {
      padding: 0 50px;
    }
  }

  @media only screen and (max-width: 560px) {
    .navigation {
      height: 30px;
    }
  }

  @media only screen and (max-width: 450px) {
    .navigation {
      padding: 0 20px;
    }
  }

  .navigation-item {
    width: max-content;
    display: flex;
    align-items: center;
    cursor: pointer;
    transition: all 0.3s;
    margin-right: 30px;
  }

  @media only screen and (max-width: 480px) {
    .navigation-item {
      margin-right: 10px;
    }
  }

  .navigation-item:hover {
    color: #1a4cff;
  }

  .navigation-icon {
    font-size: 25px;
  }

  @media only screen and (max-width: 1700px) {
    .navigation-icon {
      font-size: 23px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .navigation-icon {
      font-size: 21px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .navigation-icon {
      font-size: 18px;
    }
  }

  @media only screen and (max-width: 560px) {
    .navigation-icon {
      font-size: 14px;
    }
  }

  .navigation-label {
    font-size: 20px;
    font-weight: bold;
    margin-left: 5px;
  }

  @media only screen and (max-width: 1700px) {
    .navigation-label {
      font-size: 18px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .navigation-label {
      font-size: 16px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .navigation-label {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 560px) {
    .navigation-label {
      font-size: 12px;
    }
  }

  .input-container {
    height: 50%;
    width: 500px;
    border: 1px solid #252525;
    border-radius: 25px;
    margin-left: auto;
    padding-left: 15px;
    opacity: 0.7;
  }

  @media only screen and (max-width: 1500px) {
    .input-container {
      height: 65%;
    }
  }

  .navigation-input {
    color: #252525;
    height: 100%;
    width: calc(100% - 50px);
    outline-style: none;
    font-size: 16px;
    border: none;
    background: transparent;
  }

  @media only screen and (max-width: 1500px) {
    .navigation-input {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .navigation-input {
      font-size: 12px;
    }
  }

  @media only screen and (max-width: 700px) {
    .navigation-input {
      font-size: 10px;
    }
  }

  @media only screen and (max-width: 560px) {
    .navigation-input {
      font-size: 8px;
      transform: translateY(-3px) translateX(-5px);
      width: calc(100% - 30px);
    }
  }

  .input-icon {
    opacity: 0.9;
  }

  @media only screen and (max-width: 1500px) {
    .input-icon {
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .input-icon {
      font-size: 12px;
      margin-bottom: -3px;
    }
  }

  @media only screen and (max-width: 560px) {
    .input-icon {
      transform: translateY(-2px) translateX(-5px);
    }
  }
</style>

<nav class="navigation">
  <div
    class="navigation-item"
    on:click={() => navigate('/', { replace: true })}>
    <i class="fas fa-compass navigation-icon" />
    <span class="navigation-label">Home</span>
  </div>
  <div class="navigation-item">
    <i class="fas fa-anchor navigation-icon" />
    <span class="navigation-label">Featured</span>
  </div>
  <form class="input-container" on:submit|preventDefault={handleSubmit}>
    <i class="fas fa-search input-icon" />
    <input
      type="text"
      class="navigation-input"
      bind:value={inputNovel}
      placeholder="Search novels by name or author" />
  </form>
</nav>
