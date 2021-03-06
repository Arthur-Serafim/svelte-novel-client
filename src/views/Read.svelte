<script>
  import axios from "axios";
  import BASE_URL from "../BASE_URL.js";
  import Configurator from "../components/Configurator.svelte";
  import Footer from "../components/Footer.svelte";
  export let name;
  export let chapter;
  let spacer;
  let readContainer;
  let textContainer;
  let chapterContent;
  let theme = JSON.parse(localStorage.getItem("theme"));
  let font = localStorage.getItem("font");
  localStorage.setItem(name, chapter);
  async function getContent() {
    let response = await axios.post(`${BASE_URL}/chapter/content`, {
      link: `/${name}/`,
      chapter: `${chapter}.html`
    });

    return response.data;
  }

  let content = getContent();
  let title = name
    .split("-")
    .join(" ")
    .split("/")
    .join("");
</script>

<style>
  .read-container {
    width: 100%;
    height: 100%;
    background: #f2f2f2;
    display: flex;
    justify-content: center;
    box-sizing: border-box;
    transition: all 0.3s;
  }

  .text-container {
    width: 800px;
    border-left: 1px solid rgba(0, 0, 0, 0.2);
    border-right: 1px solid rgba(0, 0, 0, 0.2);
    padding: 0 50px;
    box-sizing: border-box;
    background: white;
    transition: all 0.3s;
    overflow: hidden;
  }

  @media only screen and (max-width: 600px) {
    .text-container {
      padding: 0 25px;
    }
  }

  @media only screen and (max-width: 400px) {
    .text-container {
      padding: 0 15px;
    }
  }

  .chapter-container {
    height: 100%;
    width: 100%;
    transition: all 0.3s;
  }

  .chapter-content {
    white-space: pre-line;
    font-size: 18px;
  }

  .chapter-title {
    font-size: 16px;
  }

  .novel {
    margin-bottom: 25px;
  }

  .load-more {
    cursor: pointer;
    color: #1a4cff;
    width: max-content;
    margin-bottom: 15px;
  }

  .load-more:hover {
    text-decoration: underline;
    text-decoration-color: #1a4cff;
  }

  .chapters-container {
    width: 100%;
    justify-content: center;
    display: flex;
    flex-direction: column;
  }

  .chapter-loading {
    background: #f2f2f2;
    display: flex;
    align-items: center;
    justify-content: center;
    height: calc(100vh - 40px);
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
  {#await content}
    <Configurator name={title} chapter="" link={name} />
    <div
      class="chapter-loading"
      style={`background: ${theme ? theme.foreground : ''}`}>
      <div class="loading">
        <div style={`border-color: ${theme ? theme.color : ''}`} />
        <div style={`border-color: ${theme ? theme.color : ''}`} />
      </div>
    </div>
  {:then data}
    <Configurator
      name={title}
      link={name}
      chapter={data.title.split(title).join('')}
      {spacer}
      {readContainer}
      {textContainer}
      {chapterContent} />
    <div
      class="read-container"
      bind:this={readContainer}
      style={`background: ${theme ? theme.foreground : ''}; font-family: ${font ? font : ''}`}>
      <div
        class="text-container"
        bind:this={textContainer}
        style={`background: ${theme ? theme.background : ''}; color: ${theme ? theme.color : ''}`}>
        <div class="chapter-container">
          <div class="novel">
            <h2 class="chapter-title">{data.title.split(title).join('')}</h2>
            <div
              class="chapter-content"
              bind:this={chapterContent}
              style={`font-size: ${localStorage.size ? localStorage.size : ''}px`}>
              {data.content}
            </div>
            <hr />
          </div>
          <div class="chapters-container">
            {#if data.previousLink}
              <a
                href={`/${name}/${data.previousLink.split('.html').join('')}`}
                class="load-more">
                Previous Chapter
              </a>
            {/if}
            {#if data.nextLink}
              <a
                href={`/${name}/${data.nextLink.split('.html').join('')}`}
                class="load-more">
                Next Chapter
              </a>
            {/if}
          </div>
        </div>
      </div>
    </div>
    <div
      class="spacer"
      bind:this={spacer}
      style={`background: ${theme ? theme.foreground : '#f2f2f2'}; height: 25px`} />
  {:catch error}
    {error}
  {/await}
</div>
