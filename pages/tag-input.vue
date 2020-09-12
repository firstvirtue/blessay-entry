<template>
  <div class="wrapper">

    <div class="tag-container" :class="{'is-popup': isPopup}">
      <div class="tag-input" @click="handleClickTagContainer">
        <span v-for="item in currentTags" :key="item.id" class="tag">
          {{item.name}}
          <span class="close-btn" @click="handleRemoveTag(item)">x</span>
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
    handleRemoveTag(item) {
      const index = this.currentTags.indexOf(item);
      if(index > -1) {
        this.currentTags.splice(index, 1);
      }
    },
    handleKeyDownTagInput(e) {
      // console.log(e);
      switch (e.keyCode) {
        case 13:
          e.preventDefault();
          const inputValue = e.target.value;
          console.log('enter', inputValue)
          // [TODO] 태그 삽입
          if(inputValue.length > 0) {
            const item = {
              id: this.tags.length + 1,
              name: inputValue
            };
            this.tags.push(item);
            this.currentTags.push(item);
            this.$refs.tagInput.value = '';
          }
          // [TODO] 태그 비교 체크
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
  .wrapper {
    max-width: 1080px;
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

    .close-btn {
      display: none;
      cursor: pointer;
    }

    &--current {
      min-width: 100px;
      display: inline-block;
    }
  }

  .is-popup {
    .tag {
      .close-btn {
        display: inline-block;
      }
    }
  }
</style>
