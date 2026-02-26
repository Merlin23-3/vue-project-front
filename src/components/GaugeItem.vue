<template>
    <div class="gauge-item">
        <div class="gauge-chart" ref="chartDomRef"></div>
        <div class="gauge-title">{{ dataItem.name }}</div>
    </div>
</template>

<script setup>
import { inject, onMounted, onUnmounted, ref, watch, nextTick } from 'vue';

const $echarts = inject('echarts');

console.log('$echarts injected:', !!$echarts);

const props = defineProps({
    dataItem: {
        type: Object,
        required: true
    }
});

const chartDomRef = ref(null);
let chart = null;


const percentValue = () => {
    const item = props.dataItem;
    return ((item.value - item.min) / (item.max - item.min) * 100).toFixed(2);
};

// 初始化图表
const initChart = async () => {
    await nextTick(); 
    if (!chartDomRef.value) {
        console.warn('chartDomRef is null');
        return;
    }
    if (!$echarts) {
        console.error('ECharts not available');
        return;
    }
    if (chart) chart.dispose();

    const item = props.dataItem;
    const option = {
        series: [{
            type: 'gauge',
            center: ['50%', '50%'],
            radius: '80%',
            startAngle: 90,
            endAngle: -270,
            pointer: { show: false },
            progress: {
                show: true,
                roundCap: true,
                itemStyle: { color: item.color }
            },
            axisLine: {
                lineStyle: {
                    width: 15,
                    color: [[1, 'rgba(255,255,255,0.1)']]
                }
            },
            splitLine: { show: false },
            axisTick: { show: false },
            axisLabel: { show: false },
            data: [{
                value: percentValue(),
                name: item.name,
                title: {
                    offsetCenter: ['0%', '-30%'],
                    color: '#fff',
                    fontSize: 12
                },
                detail: {
                    offsetCenter: ['0%', '-20%'],
                    color: item.color,
                    fontSize: 14,
                    fontWeight: 'bold',
                    width: 40,
                    height: 16,
                    borderColor: 'rgba(255,255,255,0.2)',
                    borderRadius: 8,
                    borderWidth: 1,
                    formatter: '{value}%',
                    valueAnimation: true
                }
            }]
        }]
    };

    chart = $echarts.init(chartDomRef.value);
    chart.setOption(option);
    console.log(`Gauge ${item.name} initialized`);
    
    // 监听窗口大小变化
    window.addEventListener('resize', handleResize);
};

const handleResize = () => {
    if (chart) chart.resize();
};

// 更新图表数值
const updateChart = () => {
    if (!chart) {
        console.warn('Chart not initialized, reinitializing...');
        initChart();
        return;
    }
    chart.setOption({
        series: [{
            data: [{ value: percentValue() }]
        }]
    });
};

// 对外暴露更新
defineExpose({
    updateValue: () => {
        updateChart();
    }
});

// 监听数据变化
watch(() => props.dataItem.value, () => {
    updateChart();
});

onMounted(() => {
    initChart();
});

onUnmounted(() => {
    if (chart) {
        chart.dispose();
        chart = null;
    }
    window.removeEventListener('resize', handleResize);
});
</script>

<style scoped>
.gauge-item {
    position: relative;
    width: 100%;
    height: 100%;
    background: transparent;
    border-radius: 8px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.gauge-chart {
    width: 100%;
    height: calc(100% - 20px); /* 标题空间 */
    min-height: 0;
}

.gauge-title {
    color: #fff;
    font-size: 12px;
    text-align: center;
    width: 100%;
    height: 20px;
    line-height: 20px;
    text-shadow: 0 0 5px rgba(0, 0, 0, 0.8);
    background: transparent;
}

/* 确保 canvas 填满容器 */
:deep(canvas) {
    width: 100% !important;
    height: 100% !important;
}
</style>