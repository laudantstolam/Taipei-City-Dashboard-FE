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
			`${props.map_config[0].index}-${props.map_config[0].type}`
		);
		selectedIndex.value = null;
	}
}
</script>

<template>
	<!-- conditionally render the chart -->
	<div v-if="activeChart === 'BubbleChart'">
		<!-- type: apexcharts 的圖表種類。可能與本專案的命名有些不同。 -->
		<!-- options: 填入 chartOptions 物件 -->
		<!-- series: 填入 series 或 parsed series -->
		<!-- dataPointSelection: 如有地圖篩選功能應包含 -->
		<apexchart
			width="80%"
			height="300px"
			type="bubble"
			:options="chartOptions"
			:series="parseSeries.series"
			@dataPointSelection="handleDataSelection"
		>
		</apexchart>
	</div>
</template>
