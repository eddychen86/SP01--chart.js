<template>
  <div class="title text-center py-3 bg-primary bg-gradient text-white">
    <div class="container">
      <h1 id="title" class="fw-bolder">臺鐵局每日各站之進出站人數</h1>
    </div>
  </div>
  <div class="container">
    <div class="row d-flex justify-content-center">
      <div id="chartTable" class="col-lg-5 col-md-10 p-2 m-2 shadow p-3 bg-body">
        <myChart1View />
      </div>
      <div id="chartTable" class="col-lg-5 col-md-10 p-2 m-2 shadow p-3 bg-body">
        <myChart2View />
      </div>
      <div id="chartTable" class="col-lg-5 col-md-10 p-2 m-2 shadow p-3 bg-body">
        <myChart3View />
      </div>
      <div id="chartTable" class="col-lg-5 col-md-10 p-2 m-2 shadow p-3 bg-body">
        <myChart4View />
      </div>
    </div>
  </div>
</template>


<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import myChart1View from './components/myChart1View.vue';
import myChart2View from './components/myChart2View.vue';
import myChart3View from './components/myChart3View.vue';
import myChart4View from './components/myChart4View.vue';

import trLine from '../trLine.json';
import staName from '../staName.json';
import date from '../date.json';

// 車站選項 API
axios
	.get("./staName.json")
	.then((res) => {
		const datas = res.data
		$(() => {
			datas.map((data) => {
				$("#staName1").append('<option value="' + data.staName + '">' + data.staName + '</option>');
				$("#staName2").append('<option value="' + data.staName + '">' + data.staName + '</option>');
				$("#staName3").append('<option value="' + data.staName + '">' + data.staName + '</option>');
				$("#staName4").append('<option value="' + data.staName + '">' + data.staName + '</option>');
			});
		});
	});

// 天數選項 API
axios
	.get("./date.json")
	.then((res) => {
		const datas = res.data
		$(() => {
			datas.map((data) => {
				$("#staDateS1").append('<option value="' + data.trnOpDate + '">' + data.trnOpDate + '</option>');
				$("#staDateE1").append('<option value="' + data.trnOpDate + '">' + data.trnOpDate + '</option>');

				$("#staDateS2").append('<option value="' + data.trnOpDate + '">' + data.trnOpDate + '</option>');
				$("#staDateE2").append('<option value="' + data.trnOpDate + '">' + data.trnOpDate + '</option>');

				$("#staDateS3").append('<option value="' + data.trnOpDate + '">' + data.trnOpDate + '</option>');
				$("#staDateE3").append('<option value="' + data.trnOpDate + '">' + data.trnOpDate + '</option>');

				$("#staDateS4").append('<option value="' + data.trnOpDate + '">' + data.trnOpDate + '</option>');
				$("#staDateE4").append('<option value="' + data.trnOpDate + '">' + data.trnOpDate + '</option>');
			});
		});
	});

// 幹線選項 API
axios
	.get("./trLine.json")
	.then((res) => {
		const datas = res.data
		$(() => {
			datas.map((data) => {
				$("#trLine2").append('<option value="' + data.Line + '">' + data.trLine + '</option>');
			});
		});
	});
</script>


<style>
#app {
  font-family: '微軟正黑體';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#title {
  letter-spacing: 5px;
}
#chartTable {
  border-radius: 10px;
  border: 1px solid #eee;
  overflow: hidden;
}
#dropdown-menu {
  font-size: 5px;
}
select {
  padding: 1px;
}
.searchBtn {
  border-radius: 100px !important;
  transform: scale(0.7);
}
</style>
