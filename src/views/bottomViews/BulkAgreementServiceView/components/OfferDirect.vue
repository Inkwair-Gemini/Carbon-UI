<template>
  <div class="left">
    <a-form :model="directionalOfferFormModel" :label-col="{ span: 7 }" :wrapper-col="{ span: 15 }">
      <a-form-item label="标的物代码">
        <a-input v-model:value="directionalOfferFormModel.subjectMatterCode"/>
      </a-form-item>
      <a-form-item label="标的物名称">
        <a-input v-model:value="directionalOfferFormModel.subjectMatterName" />
      </a-form-item>
      <a-form-item label="账户类型">
<!--        <a-select v-model:value="directionalOfferFormModel.accountType">-->
<!--          &lt;!&ndash; 添加选项 &ndash;&gt;-->
<!--          <a-select-option value="资金账户">资金账户</a-select-option>-->
<!--          <a-select-option value="配额账户">配额账户</a-select-option>-->
<!--        </a-select>-->
        <a-input v-model:value="directionalOfferFormModel.accountType"/>
      </a-form-item>
      <a-form-item label="报价账号">
        <a-input v-model:value="directionalOfferFormModel.account"/>
      </a-form-item>
      <a-form-item label="买卖方向">
        <a-radio-group
            v-model:value="directionalOfferFormModel.flowType"
            @change="updateFlow"
        >
          <a-radio value="卖出">卖出</a-radio>
          <a-radio value="买入">买入</a-radio>
        </a-radio-group>
      </a-form-item>
<!--      <a-form-item-->
<!--          label="可用数量"-->
<!--          v-show="directionalOfferFormModel.flowType === '卖出'"-->
<!--      >-->
<!--        <a-input-->
<!--            v-model="directionalOfferFormModel.available"-->
<!--            suffix="吨"-->
<!--            disabled-->
<!--        />-->
<!--      </a-form-item>-->
<!--      <a-form-item-->
<!--          label="可用资金"-->
<!--          v-show="directionalOfferFormModel.flowType === '买入'"-->
<!--      >-->
<!--        <a-input-->
<!--            v-model:value="directionalOfferFormModel.available"-->
<!--            suffix="元"-->
<!--            disabled-->
<!--        />-->
<!--      </a-form-item>-->

      <a-form-item label="价格">
        <a-input v-model:value="directionalOfferFormModel.price" suffix="元"/>
      </a-form-item>
      <a-form-item label="数量">
        <a-input v-model:value="directionalOfferFormModel.amount" suffix="吨"/>
      </a-form-item>
      <a-form-item label="定向用户">
        <a-input v-model:value="directionalOfferFormModel.directionClient"/>
      </a-form-item>
      <div class="buttonGroup">
        <button @click="submitForm">提交</button>
        <button @click="clearForm">清空</button>
      </div>
    </a-form>
  </div>
  <div class="right">
    <DirectionOfferQuery></DirectionOfferQuery>
  </div>
</template>
<script setup>
import {nextTick, ref} from "vue";
import DirectionOfferQuery from "./DirectionOfferQuery.vue";
import AxiosInstance from "@/utils/axiosInstance";
import axios from "axios";

const directionalOfferFormModel = ref({
  time: "",
  subjectMatterCode: "",
  subjectMatterName: "",
  accountType: "",
  account: "",
  flowType: "",
  price: "",
  amount: "",
  directionClient: "",
  operatorCode: "",
  status: "",
  // 其他表单项
});

const updateFlow = () => {
  nextTick(() => {
    console.log("用户点击了" + directionalOfferFormModel.value.flowType);
  });
};

const submitForm = () => {
  directionalOfferFormModel.value.status = "未成交";
  directionalOfferFormModel.value.operatorCode = localStorage.getItem("operatorCode");
  // 提交表单
  AxiosInstance
      .post(`/bulkAgreement/directionOffer`,
          // 表单数据
          directionalOfferFormModel.value
      )
      .then((res) => {
        let message = res.data.message;
        alert(message);
        console.log(res);
      })
      .catch((err) => {
        console.log(err);
      });
};

const clearForm = () => {
  // 清空表单
  for (let key in directionalOfferFormModel.value) {
    directionalOfferFormModel.value[key] = "";
  }
};
</script>

<style scoped>

.left {
  float: left;
  padding: 10px;
  height: 40.8vh;
  box-sizing: border-box;
  overflow: auto;
  background: #eceff6;
  border-top: 2px solid #a8b7d3;
  border-right: 2px solid #a8b7d3;
}

.right {
  float: right;
  width: 76%;
}

.buttonGroup {
  position: relative;
  left: 8vh;
  width: 25vh;
  display: flex;
  gap: 1vh;
  text-align: center;
}

button {
  margin: 5px;
  padding: 8px 13px;
  font-size: 14px;
  border: none;
  border-radius: 5px;
  background-color: #d4cfcc;
}

button:hover, button.active {
  color: white;
  background-color: #17294f;
  cursor: pointer;
}

</style>
