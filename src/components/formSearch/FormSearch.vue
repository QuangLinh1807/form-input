<template>
  <div class="form-search">
    <div class="form-search-header">
      <div class="form-search-header-title">データ項目を追加する</div>
      <div class="form-search-header-close"><CloseIcon /></div>
    </div>
    <div class="form-search-body">
      <div>
        <InputSearch
          :data="dataList"
          :result="result"
          @deleteTitle="deleteInformationDetail"
        />
      </div>
      <div class="form-search-body-select">
        <ShareMenus :data="dataList" />
        <FormCheckbox :data="dataList" @checked="onCheck" />
      </div>
    </div>
    <div class="form-search-footer">
      <button class="btn form-search-footer__btn__cancel" @onClick="cancelForm">
        キャンセル
      </button>
      <button class="btn form-search-footer__btn__submit" @click="submitForm">
        追加する
      </button>
    </div>
  </div>
</template>

<script>
import CloseIcon from "../Icons/CloseIcon.vue";
import InputSearch from "./InputSearch.vue";
import ShareMenus from "./ShareMenus.vue";
import FormCheckbox from "./FormCheckbox.vue";

export default {
  name: "App",
  data: function () {
    return {
      dataList: [
        {
          id: 1,
          title: "基本情報",
        },
        {
          id: 2,
          title: "発令",
          informationList: [
            {
              idInformation: 1,
              titleInformation: "採用情報",
            },
            {
              idInformation: 2,
              titleInformation: "学歴情報",
            },
            {
              idInformation: 3,
              titleInformation: "職歴情報",
              informationDetail: [
                {
                  informationDetailId: 1,
                  name: "業種",
                },
                {
                  informationDetailId: 2,
                  name: "職歴役職",
                },
                {
                  informationDetailId: 3,
                  name: "職歴職種",
                },
                {
                  informationDetailId: 4,
                  name: "退職理由",
                },
                {
                  informationDetailId: 5,
                  name: "退職理由",
                },
                {
                  informationDetailId: 6,
                  name: "退職理由",
                },
              ],
            },
            {
              idInformation: 4,
              titleInformation: "採用時人事コメント",
            },
            {
              idInformation: 5,
              titleInformation: "入社時人事コメント入社時人事コメント",
            },
          ],
        },
        {
          id: 3,
          title: "資格情報",
        },
      ],
      result: [],
      selected: [],
    };
  },
  components: { CloseIcon, InputSearch, ShareMenus, FormCheckbox },
  watch: {},
  methods: {
    onCheck(val) {
      this.selected = val;
    },
    submitForm() {
      console.log(1111);
      this.selected?.map((itemSelected) => {
        if (itemSelected) {
          let dataListCheckbox = [];
          this.dataList?.map((item) => {
            return item.informationList?.map((itemInformation) => {
              return itemInformation.informationDetail?.map(
                (itemInformationDetail) => {
                  dataListCheckbox.push(itemInformationDetail);
                }
              );
            });
          });
          dataListCheckbox.filter((item) => {
            if (item.informationDetailId === itemSelected) {
              this.result.includes(item) ? "" : this.result.push(item);
            }
          });
        }
      });
    },
    cancelForm() {
      this.selected = [];
    },
    deleteInformationDetail(id) {
      const indexItem = this.result.findIndex(
        (item) => item.informationDetailId === id
      );
      if (indexItem >= 0) {
        this.result.splice(indexItem, 1);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.form-search {
  border: 1px solid #dcdcdc;
  width: 640px;
  height: 628px;
  margin: auto;
  align-items: flex-start;
  background: #ffffff;
  border-radius: 4px;
  box-shadow: 0px -5px 4px #617d98;
  .form-search-header {
    display: flex;
    justify-content: space-between;
    // width: 100%;
    margin: 32px 32px 24px;
    .form-search-header-title {
      font-weight: 500;
      font-size: 24px;
      color: #333333;
    }
    .form-search-header-close {
      /deep/svg {
        width: 18px;
        height: 18px;
      }
    }
  }
  .form-search-body {
    margin: 0 32px 32px 24px;
    .form-search-body-select {
      display: flex;
      justify-content: space-between;
      margin-top: 16px;
    }
  }
  .form-search-footer {
    height: 56px;
    background: #f7f7f7;
    justify-content: flex-end;
    align-items: center;
    padding: 12px 32px 12px 0px;
    display: flex;
    .btn {
      width: 96px;
      height: 36px;
      font-weight: 700;
      font-size: 16px;
      align-items: center;
    }
    &__btn__cancel {
      background: #f7f7f7;
      color: #333333;
      margin-left: 10px;
    }
    &__btn__submit {
      background: #dae2ec;
      border-radius: 4px;
      border: none;
      color: #617d98;
    }
  }
}
</style>
