<template>
  <div
    class="multi-select"
    :id="id"
    :class="['anchor-search', isFocusClass]"
    :style="{ border: hasSearch ? '' : 'none' }"
    v-click-outside="onClickOutside"
  >
    <div v-if="hasSearch" class="search-icon">
      <SearchIcon />
    </div>
    <div class="list-selected">
      <div
        class="selected-item"
        v-for="(item, index) in options.filter((e) =>
          listSelected.includes(e[keyId])
        )"
        :key="index + listHashtagBook.length"
      >
        <div class="text-selected" :title="item[keyName]">
          #{{ item[keyName] }}
        </div>
        <div class="close-icon" @click="deleteHashtag(item)">
          <CloseIconBold />
        </div>
      </div>
      <div
        class="selected-item"
        v-for="(item, index) in listHashtagBook"
        :key="index"
      >
        <div class="text-selected">#{{ item.hashtag_name }}</div>
        <div
          class="close-icon"
          @click="deleteHashtag(item)"
          v-show="accountId === item.created_by_pp_employee_id"
        >
          <CloseIconBold />
        </div>
      </div>
      <div
        v-if="hasSearch"
        class="d-flex search-input"
        :style="{ flex: '1 1 0%' }"
      >
        <input
          type="text"
          :placeholder="placeholder"
          :readonly="disabled"
          v-model="textSearch"
          @input="onSearchItem"
          @focus="onFocusSearch"
          @keydown.enter="onAddItem"
          :class="[isFocusClass]"
        />
      </div>
    </div>
    <div id="listOption" class="list-option" v-show="toggleOptions">
      <div
        v-for="(option, indexOption) in optionsRender"
        :key="indexOption"
        class="list-option__element"
        :class="{ selected: onSelected(option) }"
        @click="handleSelected(option)"
        :title="option[keyName]"
      >
        {{ option[keyName] }}
      </div>
    </div>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import CloseIconBold from "@/share/components/icons/CloseIconBold.vue";
import SearchIcon from "@/share/components/icons/SearchIcon.vue";

export default {
  directives: {
    ClickOutside,
  },
  props: {
    value: {
      type: Array,
      required: true,
    },
    options: {
      type: Array,
      required: true,
      default: () => [],
    },
    disabled: {
      type: Boolean,
      required: false,
    },
    placeholder: {
      type: String,
    },
    keyId: {
      type: String,
      default: "id",
    },
    keyName: {
      type: String,
      default: "value",
    },
    id: {
      type: String,
      default: "multiSelect",
    },
    hasSearch: {
      type: Boolean,
      default: true,
    },
    listHashtagOfBook: {
      type: Array,
    },
    onlySelectOne: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    CloseIconBold,
    SearchIcon,
  },
  data() {
    return {
      listSelected: [],
      optionsRender: [],
      textSearch: "",
      toggleOptions: false,
      isFocus: false,
      accountId: this.$store.state["main-page"].myaccount.id,
      listHashtagBook: [],
    };
  },
  created() {
    this.listSelected = JSON.parse(JSON.stringify(this.value));
    this.optionsRender = JSON.parse(JSON.stringify(this.options));
  },
  watch: {
    value() {
      this.listSelected = JSON.parse(JSON.stringify(this.value));
    },
    options() {
      this.optionsRender = JSON.parse(JSON.stringify(this.options));
    },
    listHashtagOfBook(newValue) {
      const checkId = {};
      const optionHashtag = [];
      for (let i = 0; i < newValue.length; i++) {
        if (!checkId[newValue[i].hashtag_name]) {
          checkId[newValue[i].hashtag_name] = newValue[i].hashtag_name;
          optionHashtag.push(newValue[i]);
        } else if (newValue[i].created_by_pp_employee_id === this.accountId) {
          const index = optionHashtag
            .map((item) => item.hashtag_name)
            .indexOf(newValue[i].hashtag_name);
          optionHashtag[index].created_by_pp_employee_id = this.accountId;
        }
      }
      this.listHashtagBook = optionHashtag;
    },
  },
  computed: {
    isFocusClass() {
      return this.isFocus ? "multi-select--focus" : null;
    },
    isCheckInput() {
      return this.options.length === 0;
    },
  },
  mounted() {
    setTimeout(() => {
      this.setPositionOptions();
    }, 1);
  },
  methods: {
    setPositionOptions() {
      const domParent = document.getElementById(this.id);
      const listOption = document.querySelectorAll(`#${this.id} #listOption`);
      if (listOption && listOption[0]) {
        listOption[0].style.top = domParent.offsetHeight + "px";
      }
    },
    handleSelected(option) {
      const listId = this.listSelected;
      // remove item selected
      if (listId.includes(option[this.keyId])) {
        const index = listId.indexOf(option[this.keyId]);
        // case only select one
        if (this.onlySelectOne) {
          this.listSelected = [];
          this.toggleOptions = false;
        } else {
          this.listSelected.splice(index, 1);
        }
      }
      // add item selected
      else {
        // case only select one
        if (this.onlySelectOne) {
          this.listSelected = [option[this.keyId]];
          this.toggleOptions = false;
        } else this.listSelected.push(option[this.keyId]);
      }
      setTimeout(() => {
        this.textSearch = "";
        this.setPositionOptions();
      }, 1);
      const hashTagName = option.hashtag_name;
      this.$emit("change", hashTagName);
    },
    onSelected(option) {
      if (!this.listSelected?.length) return false;
      return this.listSelected.includes(option[this.keyId]);
    },
    onSearchItem(e) {
      if (e.target.value)
        this.optionsRender = this.options.filter((item) =>
          item[this.keyName].includes(e.target.value)
        );
      else {
        this.optionsRender = this.options;
      }
    },
    onFocusSearch() {
      if (!this.hasSearch) return;
      this.isFocus = true;
      this.toggleOptions = true;
      setTimeout(() => {
        this.setPositionOptions();
      }, 1);
    },
    onAddItem(event) {
      if (event.target.value.trim()) {
        this.$emit("addItem", event.target.value);
        this.textSearch = "";
        this.toggleOptions = false;
      }
    },
    onClickOutside() {
      this.isShowListSearch = false;
      this.toggleOptions = false;
      this.isFocus = false;
      setTimeout(() => {
        this.setPositionOptions();
      }, 1);
    },
    handleDeleteItem(optionSelected) {
      const listId = this.listSelected;
      // remove item selected
      if (listId.includes(optionSelected[this.keyId])) {
        const index = listId.indexOf(optionSelected[this.keyId]);
        this.listSelected.splice(index, 1);
      }
      this.$emit("change", this.listSelected);
      setTimeout(() => {
        this.setPositionOptions();
      }, 1);
    },
    deleteHashtag(item) {
      this.toggleOptions = false;
      this.$emit("deleteHashtag", item);
    },
  },
};
</script>

<style lang="scss" scoped>
.multi-select {
  position: relative;
  min-height: 40px;
  padding: 3px 12px;
  .selected {
    background: #627d98;
    color: #fff;
  }
  .list-option {
    background: #f0f4f8;
    color: #617d98;
    font-size: 12px;
    left: 0;
    padding: 10px 0;
    position: absolute;
    top: 40px;
    width: 100%;
    z-index: 1;
    border-radius: 4px;
    border: 1px solid #dcdcdc;
    max-height: 200px;
    overflow: auto;
    &__element {
      padding: 10px 40px;
      height: 36px;
      min-height: 36px;
      cursor: pointer;
      max-width: 100%;
      overflow: hidden;
      text-overflow: ellipsis;
      word-break: keep-all;
      &:hover {
        background: #627d98;
        color: #fff;
      }
    }
  }
  &--focus {
    background-color: #f0f4f8 !important;
    color: #486581;
  }
  .list-selected {
    display: flex;
    flex-wrap: wrap;
    width: calc(100% - 24px);
    .search-input {
      min-width: 168px;
      display: inline-block;
      input {
        min-width: 168px;
        min-height: 32px;
      }
    }
  }
}
.anchor-search {
  background-color: #ffffff;
  border: 1px solid #dcdcdc;
  border-radius: 4px;
  max-width: 100%;
  padding: 7px 12px;
  display: flex;
  gap: 14px;
  position: relative;
  &--on {
    border-color: #999999;
    background: #f0f4f8;
    input {
      background: #f0f4f8;
    }
  }
  input {
    border: none;
    height: unset;
    padding-left: unset;
  }
  .list-selected {
    .selected-item {
      background: #f0f4f8;
      border-radius: 4px;
      display: flex;
      padding: 6px 12px 6px 8px;
      gap: 8px;
      align-items: center;
      color: #627d98;
      font-size: 14px;
      display: inline-flex;
      margin-right: 8px;
      border: 1px solid #dbdbdb;
      height: 32px;
      max-width: 100%;
      .text-selected {
        overflow: hidden;
        text-overflow: ellipsis;
        word-break: break-all;
        -webkit-line-clamp: 1 !important;
        -webkit-box-orient: vertical !important;
        white-space: normal;
        display: -webkit-box;
      }
      div {
        // max-width: 300px;
        overflow: hidden;
        text-overflow: ellipsis;
        word-break: keep-all;
        -webkit-line-clamp: 1 !important;
        -webkit-box-orient: vertical !important;
        white-space: normal;
        @media screen and (max-width: 1432px) {
          // max-width: 200px;
        }
      }
      .close-icon {
        align-items: center;
        cursor: pointer;
        display: flex;
        margin-top: 2px;
        width: max-content;
      }
    }
  }
}
.d-flex {
  display: flex;
}
.search-icon {
  width: 24px;
  height: 24px;
  margin-top: 5px;
}
::-webkit-scrollbar {
  width: 4px;
  height: 4px;
}
::-webkit-scrollbar-track {
  background: transparent;
}
::-webkit-scrollbar-thumb {
  background: #dcdcdc;
}
::-webkit-scrollbar-thumb:hover {
  background: #627d98;
}
</style>

<style lang="scss" scoped>
@import "cydas-people-uikit/src/styles/variables.scss";
.employee-search-spinner {
  color: $mediumGrey;
  font-size: 24px;
  margin-top: -12px;
  position: absolute;
  top: 50%;
  right: 4px;
  z-index: 1;
  animation-name: kurukuru;
  animation-duration: 1500ms;
  animation-iteration-count: infinite;
  animation-fill-mode: both;
  animation-timing-function: linear;
}
@keyframes kurukuru {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
