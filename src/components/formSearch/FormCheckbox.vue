<template>
  <div class="form-checkbox">
    <ul class="multivalent__content">
      <li v-for="item in dataCheckbox" :key="item.id">
        <label class="mb-2 checkbox">
          <input
            class="form-group mr-2"
            type="checkbox"
            :value="item.informationDetailId"
            v-model="selected"
          />
          <span>{{ item.name }}</span>
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "FormCheckbox",
  data: function () {
    return {
      selected: [],
    };
  },
  props: {
    data: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  components: {},
  computed: {
    dataCheckbox() {
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
  watch: {
    selected(val) {
      this.$emit("checked", val);
    },
  },
  methods: {},
};
</script>

<style lang="scss" scoped>
.form-checkbox {
  width: 49%;
  height: 346px;
  border: 1px solid #dcdcdc;
  border-radius: 4px;
  padding: 8px;
  ul {
    padding: 16px 20px;
    list-style: none;
  }
  .multivalent__content {
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
}
</style>
