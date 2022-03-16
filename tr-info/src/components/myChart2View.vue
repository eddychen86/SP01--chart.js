<template>
  <div id="subtitle" class="m-1 text-center">
    <h3 class="fw-bolder">各幹線進出人數</h3>
    <div id="dropdown-menu">
      <div class="d-flex flex-row align-items-center justify-content-center">
        <select id="trLine2" class="m-1 border border-info rounded-3">
          <option value="" selected>選擇</option>
        </select>
        <select id="staDateS2" class="m-1 border border-info rounded-3">
          <option value="" selected>選擇</option>
        </select> ~ 
        <select id="staDateE2" class="m-1 border border-info rounded-3">
          <option value="" selected>選擇</option>
        </select>
        <fa icon="magnifying-glass" class="searchBtn p-2 btn btn-primary" @click="getInfo2(this)" />
      </div>
      <p class="text-center text-danger">※ 需要刷新網頁才能顯示選擇新的數據 ※</p>
    </div>
  </div>
  <div><canvas id="myChart2"></canvas></div>
</template>

<script setup>
import { computed, ref, defineComponent } from 'vue';
import { LineChart } from 'vue-chart-3';
import { Chart, registerables } from 'chart.js';

Chart.register(...registerables);

function getInfo2() {
  return new Promise((resolve) => {
    const trLine = $("#trLine2").val();
    console.log(trLine);
    const dateS2 = $("#staDateS2").val();
    const dateE2 = $("#staDateE2").val();
    axios.get(`https://taiwan-railway.herokuapp.com/line/${trLine}/${dateS2}/${dateE2}`).then((response) => {
      resolve(response.data);
      const resp2 = response.data;
      console.log(resp2);

      const labelCount2 = [];
      const data1Count2 = [];
      const data2Count2 = [];

      resp2.map((item2) => {
        let newItem2 = { 
          x: item2["trnOpDate"],
          y1: item2["gateInComingCnt"],
          y2: item2["gateOutGoingCnt"]
        }
        // console.log(newItem1["x"]);

        labelCount2.push(newItem2.x);
        data1Count2.push(newItem2.y1);
        data2Count2.push(newItem2.y2);
      });
      // console.log(labelCount1, data1Count1, data2Count1);
      
      const ctx2 = document.getElementById("myChart2");
      const data2 = {
        labels: labelCount2,
        datasets: [{
          data: data1Count2,
          fill: true,
          backgroundColor: "rgba(98, 54, 245, 0.2)",
          borderColor: "rgba(98, 54, 245)",
          tension: 0.1
        }]
      }
      const config2 = {
        type: "line",
        data: data2
      }
      const myChart2 = new Chart(ctx2, config2);
    });
  });
}
</script>