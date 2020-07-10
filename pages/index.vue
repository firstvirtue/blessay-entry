<template>
  <div class="container">
    <div>
      <ArticleList v-if="articles !== null" :articlesProp="articles" :baseViewPathProp="baseViewPath"/>
    </div>
  </div>
</template>

<script>
import ArticleList from '~/components/ArticleList'

export default {
  components: {
    ArticleList
  },
  data() {
    return {
      articles: null,
      baseViewPath: './post/',
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
  padding-top: 100px;
  padding-bottom: 100px;
  min-height: 100vh;
}

@media (min-width: 500px) {
  .container {
    padding-top: 150px;
  }
}
</style>
