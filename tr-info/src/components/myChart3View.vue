<template>
	<div id="subtitle" class="m-1 text-center">
		<h3 class="fw-bolder">單日各車站進出人數比例</h3>
		<div id="dropdown-menu">
			<div class="d-flex flex-row align-items-center justify-content-center">
				<select id="staName3" class="m-1 border border-info rounded-3">
					<option value="" selected>選擇</option>
				</select> 車站
				<select id="staDateS3" class="m-1 border border-info rounded-3">
					<option value="" selected>選擇</option>
				</select>
				<fa icon="magnifying-glass" class="searchBtn p-2 btn btn-primary" onclick="getInfo3(this)" />
			</div>
			<p class="text-center text-danger">※ 需要刷新網頁才能顯示選擇新的數據 ※</p>
		</div>
	</div>
	<div><canvas id="myChart3"></canvas></div>
</template>

<script setup>
import { computed, ref, defineComponent } from 'vue';
import { BarChart } from 'vue-chart-3';
import { Chart, registerables } from 'chart.js';

Chart.register(...registerables);

function getInfo3() {
	return new Promise((resolve) => {
		const sta3 = $("#staName3").val();
		const dateS3 = $("#staDateS3").val();
		axios.get(`https://taiwan-railway.herokuapp.com/${sta3}/${dateS3}`).then((response) => {
			resolve(response.data);
			const resp3 = response.data;

			const labelCount3 = [];
			const data1Count3 = [];			

			resp3.map((item3) => {
				const newItem3 = { 
					x: item3["trnOpDate"],
					y1: item3["gateInComingCnt"],
					y2: item3["gateOutGoingCnt"]
				}

				labelCount3.push(newItem3.x);
				data1Count3.push(newItem3.y1, newItem3.y2);
			});
			console.log(data1Count3);
			
			const ctx3 = document.getElementById("myChart3");
			const data3 = {
				labels: ["進站人數", "出站人數"],
				datasets: [{
					label: "進站人數",
					data: data1Count3,
					backgroundColor: ["rgba(98, 54, 245)", "rgba(255, 89, 89)"],
				}]
			}
			const config3 = {
				type: "pie",
				data: data3,
				options: {
					aspectRatio: 1.5,
					plugins: {
						legend: {
							position: "right"
						}
					}
				}
			}
			const myChart3 = new Chart(ctx3, config3);
		});
	});
}
</script>