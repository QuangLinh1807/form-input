<template>
  <div class="share-menus">{{}}</div>
</template>

<script>
export default {
  name: "ShareMenus",
  data: function () {
    return {};
  },
  props: {
    data: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  components: {},
  watch: {},
  methods: {},
};
</script>

<style lang="scss" scoped>
.share-menus {
  width: 49%;
  height: 346px;
  border: 1px solid #dcdcdc;
  border-radius: 4px;
  padding: 8px;
}
</style>
