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
		const trLine2 = $("#trLine2").val();
		const dateS2 = $("#staDateS2").val();
		const dateE2 = $("#staDateE2").val();
		axios.get(`https://taiwan-railway.herokuapp.com/line/${trLine2}/${dateS2}/${dateE2}`).then((response) => {
			resolve(response.data);
			const resp2 = response.data;

			const trline2 = [resp2.trnOpDate];
			const gateIn = [resp2.gateInTotal];
			const gateOut = [resp2.gateOutTotal * -1];
			console.log(gateIn, gateOut);

			const ctx2 = $("#myChart2");
			const data2 = {
				labels: trline2,
				datasets: [{
					label: "進站總人數",
					data: gateIn,
					backgroundColor: "rgba(98, 54, 245, 0.2)",
					borderColor: "rgba(98, 54, 245, 1)",
					borderWidth: 1
				},{
					label: "出站總人數",
					data: gateOut,
					backgroundColor: "rgba(255, 89, 89, 0.2)",
					borderColor: "rgba(255, 89, 89, 1)",
					borderWidth: 1
				}]
			}
			const tooltip2 = {
				yAlign: "bottom",
				titleAlign: "center",
				callbacks: {
					label: (context) => {
						return `${context.dataset.label} ${Math.abs(context.raw)}`
					}
				}
			};
			const config2 = {
				type: "bar",
				data: data2,
				options: {
					indexAxis: 'y',
					// maintainAspectRatio: false,
					scales: {
						x: {
							stacked: true,
							beginAtZero: true,
							ticks: {
								callback: (value) => {
									// console.log(Math.abs(value));
									return Math.abs(value);
								}
							}
						},
						y: {
							stacked: true,
						}
					},
					plugins: {
						tooltip: tooltip2
					}
				}
			}
			const myChart2 = new Chart(ctx2, config2);
		})
    .catch(err => console.log(error));
	});
}
</script>