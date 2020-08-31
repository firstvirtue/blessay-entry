<template>
  <div class="tag-wrapper">
    <div class="tag-container">
      <div class="tag-input" contenteditable="true" @focus="handleFocusTagInput" @blur="handleBlurTagInput" @keydown="handleKeyDownTagInput">
        <span v-for="item in current" :key="item.id">
          {{item.name}}
        </span>
      </div>
      <div class="tag-popup" v-show="isPopup">
        <ul class="tag-list">
          <li class="tag-item" v-for="item in tags" :key="item.id" @click="handleTagItem(item)">
            <span class="tag">{{item.name}}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isPopup: false,
      current: [

      ],
      tags: [
        { id: 1, name: '객체' },
        { id: 2, name: '함수' }
      ]
    }
  },
  methods: {
    handleKeyDownTagInput(e) {
      console.log(e);
    },
    handleFocusTagInput() {
      this.isPopup = true;
    },
    handleBlurTagInput() {
      // this.isPopup = false;
    },

    handleTagItem(item) {
      this.current.push(item);
    }
  }
}
</script>

<style lang="scss" scoped>
  .tag-wrapper {
    max-width: 1280px;
    margin: 0 auto;
    padding-top: 20rem;
  }
  .tag {
    &-container {
      // TEMP
      position: absolute;
      // top: 200px;
      // left: 200px;
      background-color: aliceblue;
    }

    &-input {
      min-height: 1em;
      min-width: 20rem;
    }

    & {

    }
  }
</style>
