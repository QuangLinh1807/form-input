<template>
  <div class="dropdown">
    <div class="dropdown__element" @click="showDropdown">
      <div class="over-select"></div>
      <select class="select-form-input form-group">
        <option>{{ title }}</option>
      </select>
    </div>
    <div class="multivalent" v-if="show">
      <ul class="multivalent__content">
        <li v-for="item in data" :key="item.id">
          <input
            class="form-group mr-2"
            type="checkbox"
            :value="item.id"
            v-model="selected"
          />
          <label :for="item.id">{{ item.text }}</label>
        </li>
      </ul>
      <ul class="multivalent__btn">
        <button
          :class="!selected ? 'disableBtn' : 'noDisableBtn'"
          class="multivalent__btn__submit"
          @click="submit"
        >
          Đồng ý
        </button>
        <button class="multivalent__btn__cancel" @click="cancel">Hủy</button>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  props: {
    title: {
      type: String,
      default: "Chọn tỉnh thành",
    },
    data: {
      type: Object,
      default: () => {
        return {};
      },
    },
  },
  data() {
    return {
      selected: [],
      show: false,
    };
  },
  components: {},
  methods: {
    showDropdown() {
      this.show = !this.show;
    },
    submit() {
      this.$emit("submit");
      this.show = false;
      this.selected = [];
    },
    cancel() {
      this.selected = [];
      this.show = false;
    },
  },
  watch: {
    selected(val) {
      this.$emit("checked", val);
    },
  },
};
</script>

<style lang="scss" scoped>
.dropdown {
  width: 480px;
  height: 48px;
  margin: auto;
  border: 1px solid #007bc3;
  // box-shadow: 0px 0px 8px rgb(0 123 195 / 32%);
  border-radius: 4px;

  &__element {
    position: relative;
    cursor: pointer;
  }
  .select-form-input {
    width: 100%;
    height: 46px;
    border: none;
    color: #999999;
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
.multivalent {
  position: relative;
  width: 100%;
  margin-top: -15px;

  ul {
    border: 1px solid #ddd;
    border-top: 0;
    border-radius: 0 0 3px 3px;
    left: 0px;
    padding: 8px 8px;
    top: -1rem;
    width: 100%;
    list-style: none;
    max-height: 150px;
    overflow: auto;
  }
  &__content {
    border-bottom: none !important;
  }
  &__btn {
    margin-top: -16px;
    button {
      width: 104px;
      height: 32px;
      border-radius: 4px;
      border: none;
      font-weight: 700;
      font-size: 16px;
      color: #ffffff;
      margin-right: 12px;
    }
    .disableBtn {
      background: #dcdcdc;
    }
    .noDisableBtn {
      background: #007bc3;
    }
  }
}
</style>
