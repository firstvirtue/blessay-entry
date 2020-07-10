<template>
  <div class="container">
    <ArticleView v-if="article !== null" v-bind:articleProp="article"/>
  </div>
</template>

<script>
import ArticleView from '~/components/ArticleView'

export default {
  components: {
    ArticleView
  },
  asyncData({params}) {
    return {
      articleId: params.id,
      article: null
    }
  },
  async mounted() {
    await this.$axios.get(`/api/posts/read/${this.articleId}`)
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

  @media (min-width: 500px) {
  .container {
    padding-top: 150px;
  }
}
</style>
