<template id="app">
  <div id="container">
    <div class="row">
      <div class="col-md-8 mt-5 ml-5">
        <NewInput
          :data="provinceList"
          @checked="onCheck"
          @submit="submitForm"
        />
      </div>
      <div class="col-md-8 mt-2 ml-5">
        <Result :result="result" @deleteTitle="deleteProvince" />
      </div>
      <div class="col-md-8 mt-4"></div>
    </div>
  </div>
</template>

<script>
import NewInput from "./components/NewInput.vue";
import Result from "./components/Result.vue";

export default {
  name: "App",
  data: function () {
    return {
      provinceList: [
        {
          id: 1,
          text: "Hà Nội",
        },
        {
          id: 2,
          text: "Hưng Yên",
        },
        {
          id: 3,
          text: "Tp HCM",
        },
        {
          id: 4,
          text: "Nam Định",
        },
        {
          id: 5,
          text: "Thanh Hóa",
        },
        {
          id: 6,
          text: "Nghệ An",
        },
        {
          id: 7,
          text: "Thái Bình",
        },
        {
          id: 8,
          text: "Hà Nam",
        },
        {
          id: 9,
          text: "Hưng Yên",
        },
      ],
      result: [],
      selected: [],
    };
  },
  components: {
    NewInput,
    Result,
  },
  watch: {
    result: {
      handler() {
        this.onCheck();
      },
      deep: true,
      immediate: true,
    },
  },
  methods: {
    onCheck(val) {
      this.selected = val;
    },
    submitForm() {
      this.selected?.map((itemSelected) => {
        if (itemSelected) {
          this.provinceList.filter((item) => {
            if (item.id === itemSelected) {
              this.result.includes(item) ? "" : this.result.push(item);
            }
          });
        }
      });
      // this.onCheck();
    },
    deleteProvince(id) {
      const indexItem = this.result.findIndex((item) => item.id === id);
      if (indexItem >= 0) {
        this.result.splice(indexItem, 1);
      }
    },
  },
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
