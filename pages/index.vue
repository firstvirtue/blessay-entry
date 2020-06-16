<template>
  <div class="container">
    <div>
      <!-- <logo /> -->
      <ArticleList v-if="articles !== null" :articlesProp="articles" :baseViewPathProp="baseViewPath"/>

      <!-- <h1 class="title">
        blessay-entry
      </h1>
      <h2 class="subtitle">
        엔트리 포인트
      </h2>

      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey"
        >
          GitHub
        </a>
      </div> -->
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import ArticleList from '~/components/ArticleList'

export default {
  components: {
    Logo,
    ArticleList
  },
  data() {
    return {
      articles: null,
      baseViewPath: '/essay?id=',
    }
  },
  async mounted() {
    await this.$axios.get(`/api/posts/user/firstvirtue`)
      .then(res => {
        console.log(res);
        this.articles = res.data;
      })
      .catch(err => console.log(err));
  }
}
</script>

<style>
.container {
  /* margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center; */
  padding-top: 100px;
  padding-bottom: 100px;
  min-height: 100vh;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
