<template>
  <div class="blessay" data-invert>
    <div class="wrapper">
      <article class="article">
        <p v-for="block in article.blocks" :key="block.id">
          <template v-if="block.type === 'paragraph'">
            <p v-html="block.content"></p>
          </template>
          <template v-if="block.type === 'header'">
            <h1 v-if="block.optional === '1'">{{block.content}}</h1>
            <h2 v-if="block.optional === '2'">{{block.content}}</h2>
          </template>
          <template v-else-if="block.type === 'image'">
            <img :src="block.content" :alt="block.optional">
          </template>
        </p>
      </article>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    articleProp: Object
  },
  data() {
    return {
      articleId: this.articleIdProp,
      article: this.articleProp
    }
  },
  async mounted() {
    console.log(this.articleIdProp)
    // await this.$axios.get(`/api/posts/read/${this.articleId}`)
    //   .then(res => {
    //     console.log(res.data);
    //     // this.articles = res.data;
    //     this.article = res.data;
    //   })
    //   .catch(err => console.log(err));
  }
}
</script>

<style lang="scss" scoped>

</style>
