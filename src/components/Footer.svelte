<style>
  .footer-widget {
    width: 100%;
    background-color: #56d353;
    box-sizing: border-box;
    padding: 0 300px;
    color: white;

    display: flex;
    align-items: center;
    justify-content: space-evenly;
  }

  @media only screen and (max-width: 1700px) {
    .footer-widget {
      padding: 0 250px;
      font-size: 14px;
    }
  }

  @media only screen and (max-width: 1300px) {
    .footer-widget {
      padding: 0 200px;
    }
  }

  @media only screen and (max-width: 1100px) {
    .footer-widget {
      padding: 0 150px;
      height: 30px;
      font-size: 12px;
    }
  }

  @media only screen and (max-width: 850px) {
    .footer-widget {
      padding: 0 100px;
    }
  }

  @media only screen and (max-width: 550px) {
    .footer-widget {
      padding: 0 20px;
    }
  }

  .footer-link {
    cursor: pointer;
    color: white;
  }
</style>

<footer class="footer-widget">
  <a href="/" class="footer-link">About</a>
  <a href="/" class="footer-link">Disclaimer</a>
  <a href="/" class="footer-link">Contact</a>
</footer>
