<template>
  <div class="input-form">
    <div class="input-search-title">データ項目</div>

    <div class="input-search">
      <div class="list-selected">
        <div class="search-icon">
          <SearchIcon />
        </div>
        <div
          v-show="result.length > 0"
          class="selected-item"
          v-for="item in result"
          :key="item.informationDetailId"
        >
          <div class="text-selected">{{ item.name }}</div>
          <div
            class="close-icon"
            @click="deleteTitle(item.informationDetailId)"
          >
            <CloseIcon />
          </div>
        </div>
        <div class="d-flex search-input" :style="{ flex: '1 1 0%' }">
          <input
            type="text"
            placeholder="項目名で検索する"
            :readonly="disabled"
            v-model="textSearch"
            @input="onSearchItem"
            @focus="onFocusSearch"
            @keydown.enter="onAddItem"
            :class="[isFocusClass]"
          />
        </div>
      </div>
      <!-- <div id="listOption" class="list-option" v-show="toggleOptions">
        <div
          v-for="(option, indexOption) in dataLists"
          :key="indexOption"
          class="list-option__element"
          :class="{ selected: onSelected(option) }"
          @click="handleSelected(option)"
          :title="option[keyName]"
        >
          {{ option[keyName] }}
        </div>
      </div> -->
    </div>
  </div>
</template>

<script>
import SearchIcon from "../Icons/SearchIcon.vue";
import CloseIcon from "../Icons/CloseIcon.vue";
export default {
  name: "App",
  data: function () {
    return {};
  },
  props: {
    data: {
      type: Array,
      required: true,
      default: () => [],
    },
    result: {
      type: Object,
      default: () => {
        return {};
      },
    },
    
  },
  components: { SearchIcon, CloseIcon },
  computed: {
    dataLists() {
      let dataList = [];
      this.data?.map((item) => {
        return item.informationList?.map((itemInformation) => {
          return itemInformation.informationDetail?.map(
            (itemInformationDetail) => {
              dataList.push(itemInformationDetail);
            }
          );
        });
      });
      return dataList;
    },
  },
  watch: {},
  methods: {
    deleteTitle(id) {
      this.$emit("deleteTitle", id);
    },
  },
};
</script>

<style lang="scss" scoped>
.input-form {
  margin-top: 24px;

  .input-search-title {
    font-size: 14px;
    color: #333333;
    margin-bottom: 8px;
  }

  .input-search {
    display: flex;
    width: 100%;
    max-height: 88px;
    border: 1px solid #dcdcdc;
    padding: 10px 10px;
    border-radius: 4px;
    background: #ffffff;
    overflow: auto;
    flex-wrap: wrap;
    .search-icon {
      margin-right: 10px;
    }
    .list-selected {
      display: flex;
      width: 100%;
      overflow: auto;
      flex-wrap: wrap;
      .selected-item {
        display: flex;
        width: auto;
        height: 32px;
        background: #f0f4f8;
        border: 1px solid #dcdcdc;
        border-radius: 4px;
        align-items: center;
        margin-right: 8px;
        margin-bottom: 8px;
        .text-selected {
          padding-left: 8px;
          padding-right: 8px;
          font-size: 16px;
          color: #627d98;
        }
        .close-icon {
          margin-right: 12px;
        }
      }
      .search-input {
        input {
          width: 100%;
          border: none;
          color: #486581;
        }
      }
    }
  }
}
::-webkit-scrollbar {
  width: 6px;
  border-radius: 8px;
}
::-webkit-scrollbar-track {
  background: #f1f1f1;
}
::-webkit-scrollbar-thumb {
  background: #dcdcdc;
}
::-webkit-scrollbar-thumb:hover {
  background: #dcdcdc;
}
</style>
