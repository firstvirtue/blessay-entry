<template>
  <div class="wrapper">

    <div class="tag-container" :class="{'is-popup': isPopup}">
      <div class="tag-input" @click="handleClickTagContainer">
        <div v-for="item in currentTags" :key="item.id" class="tag">
          {{item.tagname}}
          <span class="close-btn" @click="handleRemoveTag(item)">x</span>
        </div>

        <input class="tag tag--current" ref="tagInput" autocomplete="off"
          @focus="handleFocusTagInput"
          @click="handleClickTagInput"
          @blur="handleBlurTagInput"
          @keydown="handleKeyDownTagInput"
          @keyup="handleKeyUpTagInput" />
      </div>
      <div class="tag-popup" v-show="isPopup">
        <ul class="tag-list">
          <li class="tag-item" v-for="item in filteredTags" :key="item.id" @click="handleAddCurrentTagItem(item, $event)">
            <span class="tag-name">{{item.tagname}}</span>
            <div class="tag-func">
              <button class="tag-func-opener" @click="handleSwitchTagFunc(item, $event)">...</button>
              <div class="preventer" :class="{'is-open': item.isOpen}" role="presentation"></div>
              <ul class="tag-func-list" :class="{'is-open': item.isOpen}">
                <li><button class="tag-func-trigger" @click="handleRemoveStoredTag(item, $event)">삭제</button></li>
              </ul>
            </div>
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
        this.tags = res.data.map(x => {x.isOpen = false; return x; });
        this.filteredTags = this.tags;
      })
      .catch(err => console.log(err));
  },
  data() {
    return {
      isModal: false,
      isPopup: false,
      currentTags: [],
      tags: [],
      filteredTags: [],
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
              domain: 'jakel.ee',
              isOpen: false,
            };

            this.handleAddTagItem(item, (newTag) => {
              this.$refs.tagInput.value = '';
              this.handleAddCurrentTagItem(newTag);
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
    handleKeyUpTagInput(e) {
      const filter = e ? e.target.value : '';
      const filtered = this.tags.filter(x => x.tagname.match(filter));
      this.filteredTags = filtered;
    },
    handleFocusTagInput(e) {
      // console.log(e);
      this.isPopup = true;
      this.handleCheckAndHideTagFunc();
    },
    handleClickTagInput(e) {
      e.stopPropagation();
    },
    handleBlurTagInput() {
      this.$refs.tagInput.value = '';
      this.filteredTags = this.tags;
    },
    handleAddCurrentTagItem(tag, event) {
      event && event.stopPropagation();
      if(this.handleCheckAndHideTagFunc()) return;
      if(this.currentTags.some(item => item.tagname === tag.tagname)) return;

      this.currentTags.push(tag);
      this.$refs.tagInput.focus();
    },
    async handleAddTagItem(tag, callback) {

      if(this.filteredTags.some(item => item.tagname === tag.tagname)) {
        callback && callback(tag);
        return;
      }

      // [TODO] update API
      await this.$axios.post(`/api/tags`, tag)
      .then(res => {
        const newTag = res.data;
        newTag.isOpen = false;

        this.tags.push(newTag);
        callback && callback(newTag);
      })
      .catch(err => console.log(err));
    },
    handleClickTagContainer(e) {
      e.stopPropagation();
      this.isPopup = true;
      this.$refs.tagInput.focus();
    },
    async handleRemoveStoredTag(tag, event) {
      event.stopPropagation();

      const check = window.confirm(`${tag.tagname} 태그를 삭제 하시겠습니까?`);

      if(check) {
        await this.$axios.delete(`/api/tags/${tag.id}`)
        .then(res => {
          console.log(tag);
          const index = this.filteredTags.indexOf(tag);
          if(index > -1) {
            this.filteredTags.splice(index, 1);
          }

          const indexCurrent = this.currentTags.indexOf(tag);
          if(indexCurrent > -1) {
            this.currentTags.splice(indexCurrent, 1);
          }

          // [TODO] Remove all others current tags
        })
        .catch(err => console.log(err));
      }
    },
    handleSwitchTagFunc(tag, event) {
      event.stopPropagation();
      this.handleCheckAndHideTagFunc();
      tag.isOpen = !tag.isOpen;
    },
    handleCheckAndHideTagFunc() {
      let flag;
      this.filteredTags.forEach(tag => {
        if(tag.isOpen) {
          flag = true;
          tag.isOpen = false;
        }
      });
      // console.log(flag);
      return flag;
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
      display: inline-block;
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

    &-item {
      display: flex;
      justify-content: space-between;
      cursor: pointer;

      &:hover {
        background-color: blue;
      }
    }

    &-func {
      position: relative;

      &-list {
        display: none;
        position: absolute;
        top: 1.2em;
        width: 4em;
        background-color: #ffffff;
        z-index: 101;

        &.is-open {
          display: block;
        }
      }

      .preventer {
        display: none;
        position: fixed;
        width: 100vw;
        height: 100vh;
        top: 0;
        left: 0;
        z-index: 100;

        &.is-open {
          display: block;
        }
      }
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
