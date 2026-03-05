<!-- eslint-disable -->
<template>
    <div class="chart-wrapper">
        <h3 class="artistic-title">流速分布</h3>
        <div class="chart" :id="chartId"></div>
    </div>
</template>

<script setup>
import { inject, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
    chartId: {
        type: String,
        default: 'flow-chart'
    }
})

let $echarts = inject("echarts")
let myChart = null

const initChart = () => {
    const chartDom = document.getElementById(props.chartId)
    if (chartDom && $echarts) {
        if (myChart) {
            myChart.dispose()
        }
        
        myChart = $echarts.init(chartDom)
        myChart.setOption({
            title: {
                left: 'center',
                textStyle: { color: '#fff', fontSize: 12 }
            },
            grid: {
                top: '12%',
                left: '8%',
                right: '15%',
                bottom: '12%',
                containLabel: true
            },
            tooltip: { 
                trigger: 'axis',
                axisPointer: { type: 'shadow' }
            },
            legend: {
                textStyle: { color: '#fff' },
                top: 'center',
                right: '0',
                orient: 'vertical',
                itemWidth: 12,
                itemHeight: 8,
                itemGap: 15
            },
            xAxis: {
                type: 'category',
                data: ['渤海海峡', '成山头', '青岛外海', '济州岛西', '对马海峡'],
                axisLabel: { 
                    color: '#fff',
                    rotate: 15,
                    fontSize: 9
                },
                axisLine: { lineStyle: { color: '#00f2fe' } }
            },
            yAxis: {
                type: 'value',
                name: '流速(m/s)',
                nameTextStyle: { color: '#fff', fontSize: 10 },
                axisLabel: { color: '#fff', fontSize: 9 },
                splitLine: { lineStyle: { color: 'rgba(255,255,255,0.1)' } }
            },
            series: [{
                name: '表层流速',
                type: 'bar',
                data: [0.85, 1.20, 0.65, 1.05, 1.35],
                itemStyle: {
                    color: new $echarts.graphic.LinearGradient(0, 0, 0, 1, [
                        { offset: 0, color: '#90EE90' },
                        { offset: 1, color: '#32CD32' }
                    ]),
                    borderRadius: [4, 4, 0, 0]
                },
                barWidth: '35%',
                label: {
                    show: true,
                    position: 'top',
                    color: '#fff',
                    fontSize: 9,
                    formatter: function(params) {
                        return params.value + ' m/s'
                    }
                }
            }, {
                name: '底层流速',
                type: 'bar',
                data: [0.45, 0.65, 0.35, 0.55, 0.75],
                itemStyle: {
                    color: new $echarts.graphic.LinearGradient(0, 0, 0, 1, [
                        { offset: 0, color: '#AFEEEE' },
                        { offset: 1, color: '#20B2AA' }
                    ]),
                    borderRadius: [4, 4, 0, 0]
                },
                barWidth: '35%'
            }]
        })
        
        window.addEventListener('resize', handleResize)
    }
}

const handleResize = () => {
    if (myChart) {
        myChart.resize()
    }
}

let resizeObserver = null

const observeContainer = () => {
    const chartDom = document.getElementById(props.chartId)
    if (chartDom && chartDom.parentElement) {
        resizeObserver = new ResizeObserver((entries) => {
            for (const entry of entries) {
                const { width, height } = entry.contentRect
                if (width > 0 && height > 0) {
                    if (!myChart) {
                        initChart()
                    } else {
                        myChart.resize()
                    }
                }
            }
        })
        resizeObserver.observe(chartDom.parentElement)
    }
}

watch(() => props.chartId, () => {
    setTimeout(initChart, 100)
})

onMounted(() => {
    setTimeout(() => {
        const chartDom = document.getElementById(props.chartId)
        if (chartDom && chartDom.parentElement) {
            const { width, height } = chartDom.parentElement.getBoundingClientRect()
            if (width > 0 && height > 0) {
                initChart()
            }
        }
        observeContainer()
    }, 100)
})

onUnmounted(() => {
    window.removeEventListener('resize', handleResize)
    if (resizeObserver) {
        resizeObserver.disconnect()
    }
    if (myChart) {
        myChart.dispose()
    }
})
</script>

<style scoped>
@font-face {
    font-family: '演示秋鸿楷';
    src: url('/src/assets/fonts/演示秋鸿楷.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
    font-display: swap;
}

.chart-wrapper {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
}

.artistic-title {
    color: white;
    text-align: center;
    margin: 4px 0 2px 0;
    font-size: 18px;
    font-weight: 500;
    font-family: '演示秋鸿楷', '华文楷体', 'KaiTi', '楷体', 'PingFang SC', 'Microsoft YaHei', serif;
    line-height: 1.3;
    letter-spacing: 2px;
    text-shadow: 0 2px 8px rgba(0, 242, 254, 0.4);
    transform: scaleY(1.05);
    display: inline-block;
    width: 100%;
}

.chart {
    flex: 1;
    width: 100%;
    min-height: 0;
    height: calc(100% - 28px);
}
</style>