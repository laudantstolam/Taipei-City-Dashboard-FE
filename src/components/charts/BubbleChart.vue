<script setup>
import { ref } from "vue";
import { useMapStore } from "../../store/mapStore";

// 註冊四個屬性(props)
const props = defineProps([
	"chart_config",
	"activeChart",
	"series",
	"map_config",
]);
const mapStore = useMapStore();

// // 選擇性包含
// // 部分圖表包含編譯函式以確保圖表間的相容性，使同一份資料能選染多種圖表
// const parseSeries = computed(() => {
//     // Parse props.series to compatible format

//     return output
// })

// Apexcharts 設定
const temp = [
	{
		name: "PM2.5",
		data: ["11", "10", "10"],
	},
];

const temp2 = [
	[
		{
			name: "sunshine",
			data: ["4.3", "3.8", "3.3"],
		},
	],
];

const temp3 = [
	{
		name: "score",
		data: ["33.7", "24.9", "15.3"],
	},
];
const chartOptions = ref({
	chart: {
		borderRadius: 5,
		toolbar: {
			show: false,
		},
	},
	// colors: props.chart_config.color,
	// labels: props.chart_config.categories,
});

// // 選擇性包含
// // 如圖表希望支援地圖篩選則應包含
const selectedIndex = ref(null);
// props.series = props.series[0];
// console(test);
function handleDataSelection(e, chartContext, config) {
	if (!props.chart_config.map_filter) {
		return;
	}
	if (config.dataPointIndex !== selectedIndex.value) {
		mapStore.addLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`,
			props.chart_config.map_filter[0],
			props.chart_config.map_filter[1][config.dataPointIndex]
		);
		selectedIndex.value = config.dataPointIndex;
	} else {
		mapStore.clearLayerFilter(
			`${props.map_config[0].index}-$s{props.map_config[0].type}`
		);
		selectedIndex.value = null;
	}
}
// const for_one = props.series[0];
// const for_two = props.series[1];
// const for_three = props.series[2];
</script>

<template>
	<!-- conditionally render the chart -->
	test
	<div v-if="activeChart === 'BubbleChart'">
		<apexchart
			width="80%"
			height="250px"
			type="line"
			:options="chartOptions"
			:series="temp"
			@dataPointSelection="handleDataSelection"
		>
		</apexchart>
		<apexchart
			width="80%"
			height="160px"
			type="line"
			:options="chartOptions"
			:series="temp2"
			@dataPointSelection="handleDataSelection"
		>
		</apexchart>
		<apexchart
			width="80%"
			height="160px"
			type="line"
			:options="chartOptions"
			:series="temp3"
			@dataPointSelection="handleDataSelection"
		>
		</apexchart>
	</div>
</template>
