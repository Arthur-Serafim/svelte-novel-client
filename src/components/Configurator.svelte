<script>
  import { navigate } from "svelte-routing";
  import { fly } from "svelte/transition";
  export let name;
  export let link;
  export let chapter;
  export let spacer;
  export let chapterContent;
  export let readContainer;
  export let textContainer;
  let navigationContainer;
  let sideNav;
  let fontItem;
  let open = false;
  let theme = JSON.parse(localStorage.getItem("theme"));
  let fontSize = localStorage.getItem("size") || 18;

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

  let fonts = ["Nunito Sans", "Merriweather", "Montserrat"];

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
    let item;

    for (item of fontItem.children) {
      item.style.background = theme.foreground;
      item.style.color = theme.color;
    }
  }

  function handleFontChange(font) {
    localStorage.setItem("font", font);
    textContainer.style.fontFamily = font;
  }

  function handleFont(size) {
    let newSize = parseInt(fontSize) + size;
    localStorage.setItem("size", newSize);
    fontSize = newSize;
    chapterContent.style.fontSize = `${fontSize}px`;
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
    transition: all 0.3s;
  }

  .navigation-novel {
    font-weight: bold;
    font-size: 12px;
    cursor: pointer;
    white-space: nowrap;
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
    transition: all 0.3s;

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
    margin: 25px 0;
  }

  .font {
    width: max-content;
    padding: 5px;
    border: 1px solid #252525;
    margin: 0 25px 25px 0;
    cursor: pointer;
    transition: all 0.3s;
    background: #f2f2f2;
  }

  .font:hover {
    opacity: 0.8;
  }

  .font-container {
    display: flex;
    flex-wrap: wrap;
  }

  .size-container {
    display: flex;
    align-items: center;
  }

  .size-icon {
    font-size: 18px;
    margin: 0 15px;
    cursor: pointer;
  }

  .size-icon:first-child {
    margin-left: 0;
  }

  .size-icon:hover {
    color: #1a4cff;
  }

  .font-size {
    font-weight: bold;
    font-size: 18px;
  }

  .hide-overflow {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
</style>

<nav
  class="navigation-container"
  bind:this={navigationContainer}
  style={`background: ${theme ? theme.background : ''}; color: ${theme ? theme.color : ''}`}>
  <i class="icon fas fa-home" on:click={() => navigate('/')} />
  <span class="navigation-novel" on:click={() => navigate(`/novel/${link}`)}>
    {name}
  </span>
  <span class="bar">/</span>
  <div class="navigation-chapter hide-overflow">{chapter}</div>
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
              style={`background: ${theme ? theme.display : ''}; border-color: ${theme ? theme.color : ''}`}
              on:click={() => handleThemeChange(theme)} />
          {/each}
        </div>
        <div class="settings-label">Font</div>
        <div class="font-container" bind:this={fontItem}>
          {#each fonts as font}
            <div
              class="font"
              style={`background: ${theme && theme.foreground}; border-color: ${theme && theme.color}`}
              on:click={() => handleFontChange(font)}>
              <p>{font}</p>
            </div>
          {/each}
        </div>
        <div class="settings-label">Size</div>
        <div class="size-container">
          <i class="size-icon fas fa-plus" on:click={() => handleFont(+1)} />
          <span class="font-size">{fontSize}</span>
          <i class="size-icon fas fa-minus" on:click={() => handleFont(-1)} />
        </div>
      </div>
    {/if}
  {/if}
</nav>
