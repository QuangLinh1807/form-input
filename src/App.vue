<template id="app">
  <div id="container">
    <div class="row d-block">
      <FormInput />
      <FormSearch />
    </div>
  </div>
</template>

<script>
import FormInput from "./components/formInput/FormInput.vue";
import FormSearch from "./components/formSearch/FormSearch.vue";

export default {
  name: "App",
  data: function () {},
  components: {
    FormInput,
    FormSearch,
  },
  watch: {},
  methods: {},
};
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
  line-height: 24px;
}
</style>
