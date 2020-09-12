<template>
  <div class="wrapper">

    <div class="tag-container" :class="{'is-popup': isPopup}">
      <div class="tag-input" @click="handleClickTagContainer">
        <span v-for="item in currentTags" :key="item.uid" class="tag">
          {{item.value}}
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
          <li class="tag-item" v-for="item in tags" :key="item.uid" @click="handleAddCurrentTagItem(item, $event)">
            <span class="tag">{{item.value}}</span>
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
        { uid: 1, value: '객체', group: 'jake' },
        { uid: 2, value: '함수', group: 'jake' }
      ]
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
          // console.log('enter', inputValue)
          // [TODO] 태그 삽입
          if(inputValue.length > 0) {
            let item = this.tags.find(el => el.value === inputValue)
            ?? {
              uid: new Date().toString(),
              value: inputValue
            };

            this.handleAddCurrentTagItem(item, null, () => {
              this.$refs.tagInput.value = '';
            });
            this.handleAddTagItem(item);
          }

          break;
        case 8:
          if(e.target.value.length === 0) {
            if(this.currentTags.length > 0) {
              this.currentTags.pop();
            }
          }
          // console.log('remove', e.target.value.length);
          break;

        default:
          break;
      }
    },
    handleFocusTagInput(e) {
      // console.log(e);
      this.isPopup = true;
    },
    handleClickTagInput(e) {
      e.stopPropagation();
    },
    handleBlurTagInput() {
      // this.isPopup = false;
    },
    handleAddCurrentTagItem(tag, event, callback) {
      event && event.stopPropagation();
      if(this.currentTags.some(item => item.value === tag.value)) return;

      this.currentTags.push(tag);
      this.$refs.tagInput.focus();
      callback && callback();
    },
    handleAddTagItem(tag) {

      if(this.tags.some(item => item.value === tag.value)) return;
      // [TODO] API
      this.tags.push(tag);
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
