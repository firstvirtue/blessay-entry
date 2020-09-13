<template>
  <div class="wrapper">

    <div class="tag-container" :class="{'is-popup': isPopup}">
      <div class="tag-input" @click="handleClickTagContainer">
        <span v-for="item in currentTags" :key="item.id" class="tag">
          {{item.tagname}}
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
          <li class="tag-item" v-for="item in tags" :key="item.id" @click="handleAddCurrentTagItem(item, $event)">
            <span class="tag">{{item.tagname}}</span>
          </li>
        </ul>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  async mounted() {
    window.addEventListener('click', () => {
      this.isPopup = false;
    });

    const domain = 'jakel.ee';
    await this.$axios.get(`/api/tags?domain=${domain}`)
      .then(res => {
        // this.articles = res.data;
        this.tags = res.data;
      })
      .catch(err => console.log(err));
  },
  data() {
    return {
      isPopup: false,
      currentTags: [

      ],
      tags: [
        // { id: 1, value: '객체', group: 'jake' },
        // { id: 2, value: '함수', group: 'jake' }
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
            let item = this.tags.find(el => el.tagname === inputValue)
            ?? {
              tagname: inputValue,
              domain: 'jakel.ee'
            };

            this.handleAddTagItem(item, () => {
              this.$refs.tagInput.value = '';
              this.handleAddCurrentTagItem(item);
            });
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
    handleAddCurrentTagItem(tag, event) {
      event && event.stopPropagation();
      if(this.currentTags.some(item => item.tagname === tag.tagname)) return;

      this.currentTags.push(tag);
      this.$refs.tagInput.focus();
    },
    async handleAddTagItem(tag, callback) {

      if(this.tags.some(item => item.tagname === tag.tagname)) return;
      // [TODO] update API
      await this.$axios.post(`/api/tags`, tag)
      .then(res => {
        // this.articles = res.data;
        this.tags.push(tag);
        callback && callback();
      })
      .catch(err => console.log(err));
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
