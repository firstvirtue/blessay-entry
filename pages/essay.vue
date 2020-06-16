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
  data() {
    return {
      article: null
    }
  },
  async mounted() {
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
</style>
