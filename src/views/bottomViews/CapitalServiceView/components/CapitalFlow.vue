<template>
  <a-form :layout="'inline'" @submit="handleSubmit">
    <a-form-item label="&nbsp;&nbsp;资金账号">
      <a-input size="small"/>
    </a-form-item>
    <a-form-item label="出入日期">
      <a-range-picker v-model="dateRange" size="small"/>
    </a-form-item>
    <a-form-item label="划转类型">
      <a-select v-model="direction" placeholder="请选择划转类型" size="small" style="width: 150px">
        <a-select-option value="all" >1</a-select-option>
        <a-select-option value="buy">2</a-select-option>
        <a-select-option value="sell">3</a-select-option>
        <!-- 更多选项 -->
      </a-select>
    </a-form-item>
    <a-form-item>
      <button html-type="submit">搜索</button>
      <span>&nbsp;&nbsp;&nbsp;</span>
      <button @click="handleReset">清空</button>
    </a-form-item>
  </a-form>
  <div>
    <a-table :columns="columns" :data-source="data" :scroll="{y: 198 }" size="small" bordered/>
  </div>
</template>

<script setup>
import {onMounted, ref} from "vue";
import axios from "axios";
import AxiosInstance from "@/utils/axiosInstance";

const columns = [
  {
    title: "交易时间",
    dataIndex: "time",
    key: "time",
  },
  {
    title: "资金账号",
    dataIndex: "capitalAccount",
    key: "capitalAccount",
  },
  {
    title: "操作员代码",
    dataIndex: "operatorCode",
    key: "operatorCode",
  },
  {
    title: "划转类型",
    dataIndex: "type",
    key: "type",
  },
  {
    title: "发生金额",
    dataIndex: "actualAmount",
    key: "actualAmount",
  },
  {
    title: "期后余额",
    dataIndex: "endingBalance",
    key: "endingBalance",
  },
  {
    title: "期后可用余额",
    dataIndex: "endingAvailableBalance",
    key: "endingAvailableBalance",
  },
  {
    title: "流水号",//就是出入金流水的id
    dataIndex: "id",
    key: "id",
  },
];

const data = ref([]); // 从后端获取数据
// 在组件创建后立即执行
const fetchData = async () => {
  try {
    const response = await axios.get("/api/data");
    data.value = response.data;
  } catch (error) {
    console.error(error);
  }
};

onMounted(() => {
  let operatorCode= localStorage.getItem("operatorCode");
  AxiosInstance.get(`http://localhost:8800/capital/selectDepositAndWithdrawalRecord/${operatorCode}`)
      .then((res) => {
        data.value = res.data.data;
        console.log(res);
      })
      .catch((err) => {
        console.log(err);
      });
});

fetchData();
</script>


