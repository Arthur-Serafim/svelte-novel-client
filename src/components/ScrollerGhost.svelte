<script>
  export let name;
  let scroller;

  function handleScrollLeft() {
    scroller.scrollLeft -= 185;
  }

  function handleScrollRight() {
    scroller.scrollLeft += 185;
  }
</script>

<style>
  .scroller-widget {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .scroll-button {
    background: none;
    border: none;
    box-shadow: none;
    outline: none;
  }

  .arrow-icon {
    font-size: 38px;
    cursor: pointer;
    color: #252525;
    margin: 0 -15px;
    transition: all 0.3s;
  }

  @media only screen and (max-width: 1300px) {
    .arrow-icon {
      font-size: 36px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .arrow-icon {
      font-size: 34px;
    }
  }

  @media only screen and (max-width: 850px) {
    .arrow-icon {
      font-size: 30px;
    }
  }

  @media only screen and (max-width: 655px) {
    .arrow-icon {
      display: none;
    }
  }

  .arrow-icon:hover {
    color: #1a4cff;
  }

  .arrow-icon.left {
    transform: translateX(-30px);
  }

  .arrow-icon.right {
    transform: translateX(30px);
  }

  .scroll-container {
    display: flex;
    overflow: auto;
    white-space: nowrap;
    scroll-behavior: smooth;

    scrollbar-width: none;
  }

  .scroll-container::-webkit-scrollbar {
    display: none;
  }

  .scroll-item-ghost {
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-right: 50px;
  }

  @media only screen and (max-width: 1000px) {
    .scroll-item-ghost {
      margin-right: 40px;
    }
  }

  @media only screen and (max-width: 800px) {
    .scroll-item-ghost {
      margin-right: 30px;
    }
  }

  @media only screen and (max-width: 750px) {
    .scroll-item-ghost {
      margin-right: 25px;
    }
  }

  .scroll-image {
    height: 200px;
    width: 135px;
    background-color: white;
    border-radius: 15px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  @media only screen and (max-width: 1700px) {
    .scroll-image {
      height: 180px;
      width: 121px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .scroll-image {
      height: 162px;
      width: 109px;
    }
  }

  @media only screen and (max-width: 1400px) {
    .scroll-image {
      height: 146px;
      width: 98px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .scroll-image {
      height: 131px;
      width: 88px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .scroll-image {
      height: 118px;
      width: 80px;
    }
  }

  @media only screen and (max-width: 750px) {
    .scroll-image {
      height: 106px;
      width: 72px;
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
      transform: scale(0.75);
    }
  }

  @media only screen and (max-width: 750px) {
    .loading {
      transform: scale(0.5);
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

<div class="scroller-widget">
  <button class="scroll-button" on:click={handleScrollLeft}>
    <i class="arrow-icon fas fa-chevron-left left" />
  </button>
  <div class={`scroll-container-${name} scroll-container`} bind:this={scroller}>
    {#each Array(10) as novel}
      <div class="scroll-item-ghost">
        <div class="scroll-image">
          <div class="loading">
            <div />
            <div />
          </div>
        </div>
      </div>
    {/each}
  </div>
  <button class="scroll-button" on:click={handleScrollRight}>
    <i class="arrow-icon fas fa-chevron-right right" />
  </button>
</div>
