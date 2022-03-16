<template>
  <div id="subtitle" class="m-1 text-center">
    <h3 class="fw-bolder">單日各幹線進出人數加總比例</h3>
    <div id="dropdown-menu">
      <div class="d-flex flex-row align-items-center justify-content-center">
        <select id="staDateE4" class="m-1 border border-info rounded-3">
          <option value="" selected>選擇</option>
        </select>
        <fa icon="magnifying-glass" class="searchBtn p-2 btn btn-primary" @click="getInfo4(this)" />
      </div>
      <p class="text-center text-danger">※ 需要刷新網頁才能顯示選擇新的數據 ※</p>
    </div>
  </div>
  <div><canvas id="myChart4"></canvas></div> 
</template>

<script setup>
import { computed, ref, defineComponent } from 'vue';
import { BarChart } from 'vue-chart-3';
import { Chart, registerables } from 'chart.js';

Chart.register(...registerables);

function getInfo4() {
	return new Promise((resolve) => {
		const date4 = $("#staDateE4").val();
		axios.get(`https://taiwan-railway.herokuapp.com/date/${date4}`).then((response) => {
			resolve(response.data);
			const resp4 = response.data;
			// console.log(resp2);

			const labelCount4 = [];

			const total1 = [];	const total2 = [];	const total3 = [];	const total4 = [];	const total5 = [];
			const total6 = [];	const total7 = []; const total8 = [];	const total9 = []; const total10 = [];

			resp4.map((item) => {
				const newItem4 = { 
					x: item["staCode"],
					y: item["gateInComingCnt"] + item["gateOutGoingCnt"]
				}
				
				// 參考：https://www.fly.idv.tw/
				if ( newItem4.x >= 900 && newItem4.x <= 1250 ) {
					total1.push(newItem4.y);
					labelCount4.push("西部幹線-北");
				} else if ( newItem4.x >= 2110 && newItem4.x <= 2260 ) {
					total2.push(newItem4.y)
					labelCount4.push("西部幹線-海");
				} else if ( newItem4.x >= 3140 && newItem4.x <= 3350 ) {
					total3.push(newItem4.y)
					labelCount4.push("西部幹線-山");
				} else if ( newItem4.x >= 3360 && newItem4.x <= 4100 ) {
					total4.push(newItem4.y)
					labelCount4.push("西部幹線-彰雲嘉");
				} else if ( newItem4.x >= 4110 && newItem4.x <= 5110 ) {
					total5.push(newItem4.y)
					labelCount4.push("西部幹線-台南高屏");
				} else if ( newItem4.x >= 5120 && newItem4.x <= 5240 ) {
					total6.push(newItem4.y)
					labelCount4.push("南迴線");
				} else if ( newItem4.x >= 6000 && newItem4.x <= 6070 ) {
					total7.push(newItem4.y)
					labelCount4.push("東部幹線-台東");
				} else if ( newItem4.x >= 6080 && newItem4.x <= 7080 ) {
					total8.push(newItem4.y)
					labelCount4.push("東部幹線-花蓮");
				} else if ( newItem4.x >= 7090 && newItem4.x <= 7360 ) {
					total9.push(newItem4.y)
					labelCount4.push("東部幹線-宜蘭&宜蘭縣");
				} else if ( newItem4.x >= 7361 && newItem4.x <= 7390 ) {
					total10.push(newItem4.y)
					labelCount4.push("其他");
				}
			});

			const data1Count4 = [];

			function sumData(arr) {
				let sum = 0;
				arr.forEach(function(e) {
					sum += e
				});
				return sum;
			}

			data1Count4.push(
				sumData(total1), sumData(total2), sumData(total3),
				sumData(total4), sumData(total5), sumData(total6),
				sumData(total7), sumData(total8), sumData(total9),
				sumData(total10)
			);

			// console.log(data1Count2);

			const labelCount4_2 = [...new Set(labelCount4)];
			// console.log(labelCount2_2);
			
			const ctx4 = document.getElementById("myChart4");

			// function getRandomColor() {
			// 	let letters = '0123456789ABCDEF'.split('');
			// 	let color = '#';
			// 	for (let i = 0; i < 6; i++ )
			// 		color += letters[Math.floor(Math.random() * 16)];
			// 	return color;
			// }
			// let colors = [];
			// for (let i = 0; i < data1Count2.length ; i++){
			// 	colors.push(getRandomColor());
			// }
			
			const data4 = {
				labels: labelCount4_2,
				datasets: [{
					data: data1Count4,
					backgroundColor: 
					[
						"rgba(184, 216, 216)", "rgba(122, 158, 159)", "rgba(79, 99, 103)",
						"rgba(159, 172, 161)", "rgba(199, 209, 190)", "rgba(24, 32, 111)",
						"rgba(246, 170, 152)", "rgba(250, 133, 119)", "rgba(254, 95, 85)",
						"rgba(204, 232, 204)"
					],
					hoverOffset: 4
				}],
			}
			const config4 = {
				type: "doughnut",
				data: data4,
				options: {
					// 搭配 隨機變色 colors 
					// hover: {mode: null},
					aspectRatio: 1.5,
					plugins: {
						legend: {
							position: "right"
						}
					}
				}
			}
			const myChart4 = new Chart(ctx4, config4);		
		});
	});
}
</script>