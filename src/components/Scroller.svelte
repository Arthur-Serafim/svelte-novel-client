<script>
  export let completedNovels;
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

  .scroll-item {
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-right: 50px;
    width: min-content;
    width: 135px;
  }

  @media only screen and (max-width: 1700px) {
    .scroll-item {
      margin-right: 45px;
    }
  }

  @media only screen and (max-width: 1500px) {
    .scroll-item {
      margin-right: 40px;
      width: 109px;
    }
  }

  @media only screen and (max-width: 1400px) {
    .scroll-item {
      margin-right: 35px;
      width: 98px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .scroll-item {
      margin-right: 30px;
      width: 88px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .scroll-item {
      width: 80px;
    }
  }

  @media only screen and (max-width: 750px) {
    .scroll-item {
      width: 72px;
    }
  }

  .scroll-image {
    height: 200px;
    width: 135px;
    object-fit: cover;
    border-radius: 15px;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    cursor: pointer;
    transition: all 0.3s;
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

  .scroll-image:hover {
    opacity: 0.8;
  }

  .title {
    margin: 10px 0 5px 0;
    font-size: 16px;
    cursor: pointer;
  }

  @media only screen and (max-width: 1700px) {
    .title {
      font-size: 13px;
    }
  }

  @media only screen and (max-width: 1400px) {
    .title {
      font-size: 11px;
    }
  }

  @media only screen and (max-width: 1000px) {
    .title {
      font-size: 9px;
    }
  }

  @media only screen and (max-width: 800px) {
    .title {
      font-size: 7px;
    }
  }

  .title:hover {
    text-decoration: underline;
  }

  .hide-overflow {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
</style>

<div class="scroller-widget">
  <button class="scroll-button" on:click={handleScrollLeft}>
    <i class="arrow-icon fas fa-chevron-left left" />
  </button>
  <div class={'scroll-container'} bind:this={scroller}>
    {#each completedNovels as novel}
      <div class="scroll-item">
        <img
          src={novel.image}
          alt={`Image for ${novel.title}`}
          class="scroll-image" />
        <h2 class="title hide-overflow">{novel.title}</h2>
      </div>
    {/each}
  </div>
  <button class="scroll-button" on:click={handleScrollRight}>
    <i class="arrow-icon fas fa-chevron-right right" />
  </button>
</div>
