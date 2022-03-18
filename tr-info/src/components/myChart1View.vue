<template>
	<div id="subtitle" class="m-1 text-center">
		<h3 class="fw-bolder">各車站進出人數</h3>
		<div id="dropdown-menu">
			<div class="d-flex flex-row align-items-center justify-content-center">
				<select id="staName1" class="mx-1 border border-info rounded-3">
					<option value="" selected>選擇</option>
				</select> 車站
				<select id="staDateS1" class="mx-1 border border-info rounded-3">
					<option value="" selected>選擇</option>
				</select> ~
				<select id="staDateE1" class="mx-1 border border-info rounded-3">
					<option value="" selected>選擇</option>
				</select>
				<fa icon="magnifying-glass" class="searchBtn p-2 btn btn-primary" @click="getInfo1(this)" />
			</div>
			<p class="text-center text-danger">※ 需要刷新網頁才能顯示選擇新的數據 ※</p>
		</div>
	</div>
	<div><canvas id="myChart1"></canvas></div>
</template>

<script setup>
import { computed, ref, defineComponent } from 'vue';
import { BarChart } from 'vue-chart-3';
import { Chart, registerables } from 'chart.js';

Chart.register(...registerables);

function getInfo1() {
	return new Promise((resolve) => {
		const sta1 = $("#staName1").val();
		const dateS1 = $("#staDateS1").val();
		const dateE1 = $("#staDateE1").val();
		axios.get(`https://taiwan-railway.herokuapp.com/${sta1}/${dateS1}/${dateE1}`).then((response) => {
			resolve(response.data);
			const resp1 = response.data;
			
			const labelCount1 = [];
			const data1Count1 = [];
			const data2Count1 = [];

			resp1.map((item1) => {
				const newItem1 = { 
					x: item1["trnOpDate"],
					y1: item1["gateInComingCnt"],
					y2: item1["gateOutGoingCnt"]
				}
				// console.log(newItem1["x"]);

				labelCount1.push(newItem1.x);
				data1Count1.push(newItem1.y1);
				data2Count1.push(newItem1.y2);
			});
			// console.log(labelCount1, data1Count1, data2Count1);

			const ctx1 = $("#myChart1");
			const data1 = {
				labels: labelCount1,
				datasets: [{
					label: "進站總人數",
					data: data1Count1,
					fill: true,
					backgroundColor: "rgba(98, 54, 245, 0.2)",
					borderColor: "rgba(98, 54, 245)",
					tension: 0.1,
					pointStyle: 'circle',
					pointRadius: 3,
					pointHoverRadius: 7
				},{
					label: "出站總人數",
					data: data2Count1,
					fill: true,
					backgroundColor: "rgba(255, 89, 89, 0.2)",
					borderColor: "rgba(255, 89, 89)",
					tension: 0.1,
					pointStyle: 'circle',
					pointRadius: 3,
					pointHoverRadius: 7
				}]
			}
			const config1 = {
				type: "line",
				data: data1,
				options: {
					maintainAspectRatio: false
				}
			}
			const myChart1 = new Chart(ctx1, config1);
		})
		.catch(err => console.log(error));
	});
}
</script>
