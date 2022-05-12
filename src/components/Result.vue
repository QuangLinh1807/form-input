<template>
  <div class="result">
    <div class="result-element" v-for="item in result" :key="item.id">
      <div>
        {{ item.text }}
        <div @click="deleteTitle(item.id)">x</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  props: {
    result: {
      type: Object,
      default: () => {
        return {};
      },
    },
  },
  components: {},
  methods: {
    deleteTitle(id) {
      this.$emit("deleteTitle", id);
    },
  },
};
</script>

<style lang="scss" scoped>
.result {
  width: 60%;
  margin: auto;
  display: flex;
  .result-element {
    width: 86px;
    height: 24px;
    font-family: "Noto Sans";
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 24px;
    display: flex;
    align-items: center;
    text-align: center;
    color: #333333;
    flex: none;
    order: 0;
    flex-grow: 0;
  }
}
</style>
