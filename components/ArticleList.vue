<template>
  <div class="blessay">
    <ul class="blessay__list">
      <li class="blessay__item" v-for="item in articles" v-bind:key="item.id">
        <a :href="`${baseViewPath}${item.id}`">
          <div class="cont">
            <div class="cont__wrapper">
              <h3 class="tit">{{item.title}}</h3>
              <p class="desc">{{item.description}}</p>
            </div>
            <span class="date">
              {{item.created_on.substring(0,10).replace(/-/gi, '.')}}
            </span>
          </div>
        </a>
        <template v-if="$auth !== undefined">
          <div class="func" :class="{'is-active': item.isActive}">
            <button @click="onUtil(item)" class="func__opener"><span class="a11y">유틸</span></button>
            <div v-show="item.isActive" class="func__layer">
              <a :href="`/post/write?id=${item.id}`" class="func__btn">수정하기</a>
              <button class="func__btn" @click="onDelete(item)">삭제하기</button>
            </div>
          </div>
        </template>
      </li>
    </ul>
    <template v-if="articles && articles.length === 0">
      <div class="notice-recommend">
        <a href="/post/write">새로운 글을 작성해 보세요!</a>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  props: {
    articlesProp: Array,
    baseViewPathProp: String
  },
  data() {
    return {
      articles: this.articlesProp,
      baseViewPath: this.baseViewPathProp,
    }
  }
}
</script>

<style lang="scss" scoped>
a {
  display: block;
  color: #666666;
  text-decoration: none;
  text-align: left;
}

.blessay {
  &__list {
    max-width: 1280px;
    margin: 0 auto;
  }

  &__item {
    display: flex;
    padding-top: 20px;
    padding-bottom: 20px;
    border-bottom: 1px solid #f7f7f7;
  }
}

.notice-recommend {
  a {
    text-align: center;
    font-weight: bolder;
    font-size: 24px;
  }
}
</style>
