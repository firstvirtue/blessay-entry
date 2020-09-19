<template>
  <div class="wrapper">
    <Tag />
  </div>
</template>

<script>
import Tag from '~/components/Tag'
export default {
  components: {
    Tag: Tag
  }
}
</script>

<style lang="scss" scoped>
  .wrapper {
    max-width: 1080px;
    margin: 0 auto;
    padding-top: 20rem;
  }
</style>
