<template>
  <div class="container">
    <ArticleView v-if="article !== null" v-bind:articleProp="article"/>

    <div id="fb-root"></div>
    <script async defer crossorigin="anonymous" src="https://connect.facebook.net/en_GB/sdk.js#xfbml=1&version=v7.0&appId=224450648663850&autoLogAppEvents=1" nonce="bXyrxPJH"></script>
    <div class="fb-container">
      <div class="fb-comments" :data-href="articleHref" data-numposts="5" data-width="100%"></div>
    </div>
  </div>
</template>

<script>
import ArticleView from '~/components/ArticleView'

export default {
  components: {
    ArticleView
  },
  asyncData() {
    return {
      article: null
    }
  },
  async mounted() {
    console.log(this.$route.query.id);
    this.articleHref = `https://blog.jakel.ee/essay?id=${this.$route.query.id}`;
    await this.$axios.get(`/api/posts/read/${this.$route.query.id}`)
      .then(res => {
        console.log(res.data);
        // this.articles = res.data;
        this.article = res.data;
      })
      .catch(err => console.log(err));
  }
}
</script>

<style lang="scss" scoped>
  .container {
    padding-top: 100px;
    padding-bottom: 100px;
  }

  .fb-container {
    margin: 10rem auto 0;
    max-width: 630px;
    width: 90%;
  }

  @media (min-width: 540px) {
    .container {
      padding-top: 150px;
    }

    .fb-container {
      margin: 15rem auto 0;
    }

  }
</style>
