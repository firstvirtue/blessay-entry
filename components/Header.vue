<template>
  <header>
    <div class="bg" role="presentation"></div>
    <nav>
      <a href="/" class="home">Jake Lee</a>
      <div class="links">
        <a class="instagram" href="https://www.instagram.com/sangolee" target="_blank">instagram</a>
        <a class="twitter" href="https://twitter.com/sangolee" target="_blank">twitter</a>
      </div>
    </nav>
  </header>
</template>

<style lang="scss" scoped>
  header {
    position: fixed;
    width: 100%;
    box-sizing: border-box;
    z-index: 1000;
    padding-left: 6%;
    padding-right: 6%;
    height: 50px;
    transition: height 0.4s;
  }

  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    height: 100%;
    max-width: 1100px;
    margin: 0 auto;
  }

  .bg {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(#ffffff, 0.7);
    border-bottom: 1px solid #dee2e6;
    transition: opacity 0.4s;
  }

  .home {
    color: #212529;
    font-size: 1.8rem;
    font-weight: bolder;
  }

  .links {
    a {
      color: #212529;
      font-size: 1.8rem;
      text-transform: capitalize;
      font-weight: bolder;
      transition: color 0.2s;

      & + a {
        margin-left: 1em;
      }

      &.instagram {
        &:hover,
        &:focus {
          color: #E1306C;
        }
      }

      &.twitter {
        &:hover,
        &:focus {
          color: #1DA1F2;
        }
      }
    }
  }

  @media (min-width: 540px) {
    header {
      height: 72px;
    }

    .home {
      font-size: 2rem;
    }

    .links {
      a {
        font-size: 2rem;
      }
    }
  }

  @media (min-width: 1110px) {
    .bg {
      opacity: 0;
    }
  }
</style>
