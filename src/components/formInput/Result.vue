<template>
  <div class="result">
    <div class="result-element" v-for="item in result" :key="item.id">
      <div class="d-flex m-auto">
        {{ item.text }}
        <div class="ml-3" @click="deleteTitle(item.id)"><CloseIcon /></div>
      </div>
    </div>
  </div>
</template>

<script>
import CloseIcon from "../Icons/CloseIcon.vue";

export default {
  name: "ShareMenus",
  props: {
    result: {
      type: Object,
      default: () => {
        return {};
      },
    },
  },
  components: { CloseIcon },
  methods: {
    deleteTitle(id) {
      this.$emit("deleteTitle", id);
    },
  },
};
</script>

<style lang="scss" scoped>
.result {
  display: flex;
  width: 480px;
  height: 48px;
  overflow: auto;
  flex-wrap: wrap;
  margin: auto;
  border: 1px solid #007bc3;
  border-radius: 4px;
  .result-element {
    width: 134px;
    height: 32px;
    font-family: "Noto Sans";
    display: flex;
    background: #f8f8f8;
    margin: 7px;
    border-radius: 32px;
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
