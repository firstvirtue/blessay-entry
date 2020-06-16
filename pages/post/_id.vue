<template>
  <div class="container">
    <ArticleView v-if="article !== null" :articleProp="this.article"/>
  </div>
</template>

<script>
import ArticleView from '~/components/ArticleView'

export default {
  components: {
    ArticleView
  },
  asyncData({ params }) {
    return {
      articleId: params.id,
      article: {}
    }
  },
  async mounted() {
    console.log(this.articleId);
    await this.$axios.get(`/api/posts/read/${this.articleId}`)
      .then(res => {
        console.log(res.data);
        // this.articles = res.data;
        this.article = res.data;
      })
      .catch(err => console.log(err));
  },
  validate({ params }) {
    // return /^\d+$/.test(params.id);
    return params.id;
  }
}
</script>

<style lang="scss" scoped>
  .container {
    padding-top: 100px;
    padding-bottom: 100px;
  }
</style>
