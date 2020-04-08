<script>
  import { navigate } from "svelte-routing";
  import { fly } from "svelte/transition";
  export let name;
  export let chapter;
  export let spacer;
  export let readContainer;
  export let textContainer;
  let navigationContainer;
  let sideNav;
  let open = false;
  let theme = JSON.parse(localStorage.getItem("theme"));

  let themes = [
    {
      name: "white",
      display: "#fff",
      background: "#fff",
      foreground: "#f2f2f2",
      color: "#252525"
    },
    {
      name: "black",
      display: "#191b1c",
      background: "#1f2129",
      foreground: "#000",
      color: "#c0c2cc"
    }
  ];

  function handleThemeChange(theme) {
    localStorage.setItem("theme", JSON.stringify(theme));
    navigationContainer.style.background = theme.background;
    navigationContainer.style.color = theme.color;
    sideNav.style.background = theme.background;
    sideNav.style.color = theme.color;
    readContainer.style.background = theme.foreground;
    spacer.style.background = theme.foreground;
    textContainer.style.background = theme.background;
    textContainer.style.color = theme.color;
  }
</script>

<style>
  .navigation-container {
    width: 100%;
    height: 40px;
    background: white;
    border-bottom: 1px solid rgba(0, 0, 0, 0.2);
    box-sizing: border-box;
    padding: 0 15px;
    display: flex;
    align-items: center;
  }

  .navigation-novel {
    font-weight: bold;
    font-size: 12px;
    cursor: pointer;
  }

  .navigation-novel:hover {
    text-decoration: underline;
  }

  .navigation-chapter {
    font-weight: bold;
    font-size: 12px;
  }

  .bar {
    font-weight: bold;
    margin: 0 5px;
  }

  .icon {
    font-size: 12px;
    margin-right: 10px;
    cursor: pointer;
  }

  .icon:hover {
    color: #1a4cff;
  }

  .settings {
    margin-left: auto;
    font-size: 16px;
    z-index: 100;
    position: fixed;
    right: 0;
  }

  .side-nav {
    position: fixed;
    top: 0;
    right: 0;
    background: white;
    max-width: 500px;
    height: 100vh;
    width: 100vw;
    border-left: 1px solid rgba(0, 0, 0, 0.2);

    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    padding: 25px;
  }

  .theme-holder {
    width: 100%;
    height: min-content;
    display: flex;
  }

  .theme {
    height: 50px;
    width: 50px;
    border-radius: 50%;
    border: 1px solid #252525;
    cursor: pointer;
    margin-right: 25px;
  }

  .theme:hover {
    opacity: 0.7;
  }

  .settings-label {
    margin-bottom: 25px;
  }
</style>

<nav
  class="navigation-container"
  bind:this={navigationContainer}
  style={`background: ${theme ? theme.background : ''}; color: ${theme ? theme.color : ''}`}>
  <i class="icon fas fa-home" on:click={() => navigate('/')} />
  <span class="navigation-novel">{name}</span>
  <span class="bar">/</span>
  <div class="navigation-chapter">{chapter}</div>
  {#if chapter}
    <i class="icon fas fa-cog settings" on:click={() => (open = !open)} />
    {#if open}
      <div
        class="side-nav"
        style={`background: ${theme ? theme.background : '#fff'}; color: ${theme ? theme.color : '#252525'}`}
        transition:fly={{ x: 500, duration: 500 }}
        bind:this={sideNav}>
        <div class="settings-label">Background</div>
        <div class="theme-holder">
          {#each themes as theme}
            <div
              class="theme"
              style={`background: ${theme.display}; border-color: ${theme.color}`}
              on:click={() => handleThemeChange(theme)} />
          {/each}
        </div>
      </div>
    {/if}
  {/if}
</nav>
