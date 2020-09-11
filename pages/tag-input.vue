<template>
  <div class="tag-wrapper">
    <div class="tag-container">
      <div class="tag-input" @click="handleClickTagContainer">
        <span v-for="item in currentTags" :key="item.id" class="tag">
          {{item.name}}
        </span>

        <input class="tag tag--current" ref="tagInput" autocomplete="off"
          @focus="handleFocusTagInput"
          @click="handleClickTagInput"
          @blur="handleBlurTagInput"
          @keydown="handleKeyDownTagInput" />
      </div>
      <div class="tag-popup" v-show="isPopup">
        <ul class="tag-list">
          <li class="tag-item" v-for="item in tags" :key="item.id" @click="handleTagItem($event, item)">
            <span class="tag">{{item.name}}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    window.addEventListener('click', () => {
      this.isPopup = false;
    });
  },
  data() {
    return {
      isPopup: false,
      currentTags: [

      ],
      tags: [
        { id: 1, name: '객체' },
        { id: 2, name: '함수' }
      ]
    }
  },
  watch: {
    isPopup: function() {
      console.log(this.isPopup);
    }
  },
  methods: {
    handleKeyDownTagInput(e) {
      // console.log(e);
      switch (e.keyCode) {
        case 13:
          e.preventDefault();
          console.log('enter', e.target.innerText)
          // [TODO] 태그 비교 삽입
          break;
        case 8:
          if(e.target.value.length === 0) {
            // [TODO] 마지막 태그 제거
          }
          console.log('remove', e.target.value.length);
          break;

        default:
          break;
      }
    },
    handleFocusTagInput(e) {
      console.log(e);
      this.isPopup = true;
    },
    handleClickTagInput(e) {
      e.stopPropagation();
    },
    handleBlurTagInput() {
      // this.isPopup = false;
    },

    handleTagItem(e, item) {
      e.stopPropagation();
      this.currentTags.push(item);
      this.$refs.tagInput.focus();
    },
    handleClickTagContainer(e) {
      e.stopPropagation();
      this.isPopup = true;
      this.$refs.tagInput.focus();
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
      background-color: magenta;
    }

    &--current {
      min-width: 100px;
      display: inline-block;
    }
  }
</style>
