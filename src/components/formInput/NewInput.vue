<template>
  <div class="dropdown">
    <div class="dropdown__element" @click="showDropdown">
      <div class="ml-2">{{ title }}</div>
      <div class="mr-3"><DownIcon /></div>
    </div>
    <div class="multivalent" v-if="show">
      <ul class="multivalent__content">
        <li v-for="item in data" :key="item.id">
          <label class="mb-2 checkbox">
            <input
              class="form-group mr-2"
              type="checkbox"
              :value="item.id"
              v-model="selected"
            />
            <span>{{ item.text }}</span>
          </label>
        </li>
      </ul>
      <ul class="multivalent__btn">
        <button
          :class="selected.length == 0 ? 'disableBtn' : 'noDisableBtn'"
          class="multivalent__btn__submit"
          @click="submit"
          :disabled="selected.length == 0"
        >
          Đồng ý
        </button>
        <button class="multivalent__btn__cancel" @click="cancel">Hủy</button>
      </ul>
    </div>
  </div>
</template>

<script>
import DownIcon from "../Icons/DownIcon.vue";
export default {
  name: "NewInput",
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
  components: { DownIcon },
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
  border-radius: 4px;

  &__element {
    position: relative;
    cursor: pointer;
    width: 100%;
    height: 46px;
    border: none;
    color: #999999;
    margin-top: 10px;
    display: flex;
    justify-content: space-between;
  }
}
//style scroll
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
  margin-top: -8px;
  background: #fff;
  z-index: 1;
  ul {
    border: 1px solid #ddd;
    border-top: 0;
    border-radius: 0 0 3px 3px;
    left: 0px;
    padding: 16px 20px;
    top: -1rem;
    width: 100%;
    list-style: none;
    max-height: 282px;
    overflow: auto;
  }
  &__content {
    border-bottom: none !important;
    // style checkbox
    .checkbox {
      display: inline-flex;
      cursor: pointer;
      position: relative;
      span {
        margin-top: -3px;
      }
      input {
        height: 16px;
        width: 16px;
        -webkit-appearance: none;
        -moz-appearance: none;
        -o-appearance: none;
        appearance: none;
        border: 1px solid #34495e;
        border-radius: 4px;
        outline: none;
        transition-duration: 0.3s;
        cursor: pointer;
      }
      input:checked {
        border: 1px solid #fff;
        background-color: #45d1c9;
        z-index: 10;
      }
      input:checked + span::before {
        content: "\2713";
        display: block;
        text-align: center;
        color: #fff;
        position: absolute;
        left: 2px;
        top: -3px;
        z-index: 100;
      }
      input:active {
        border: 2px solid #34495e;
      }
    }
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
    &__cancel {
      color: #007bc3 !important;
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
