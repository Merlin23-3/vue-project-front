<!-- eslint-disable -->
<template>
    <div class="chart-wrapper">
        <h3 class="artistic-title">波高分布</h3>
        <div class="chart" :id="chartId"></div>
    </div>
</template>

<script setup>
import { inject, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
    chartId: {
        type: String,
        default: 'wave-chart'
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
                data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
                axisLabel: { 
                    color: '#fff',
                    rotate: 30,
                    interval: 0,
                    fontSize: 9
                },
                axisLine: { lineStyle: { color: '#00f2fe' } }
            },
            yAxis: {
                type: 'value',
                name: '波高(m)',
                nameTextStyle: { color: '#fff', fontSize: 10 },
                axisLabel: { color: '#fff', fontSize: 9 },
                splitLine: { lineStyle: { color: 'rgba(255,255,255,0.1)' } },
                nameLocation: 'middle',
                nameGap: 30
            },
            series: [{
                name: '有效波高',
                type: 'line',
                data: [1.2, 1.1, 1.3, 1.5, 1.8, 2.1, 2.5, 2.8, 2.3, 1.9, 1.5, 1.3],
                lineStyle: {
                    width: 3,
                    color: new $echarts.graphic.LinearGradient(0, 0, 0, 1, [
                        { offset: 0, color: '#E0FFFF' },
                        { offset: 1, color: '#40E0D0' }
                    ])
                },
                itemStyle: {
                    color: new $echarts.graphic.LinearGradient(0, 0, 0, 1, [
                        { offset: 0, color: '#E0FFFF' },
                        { offset: 1, color: '#40E0D0' }
                    ])
                },
                label: {
                    show: true,
                    position: 'top',
                    color: '#fff',
                    fontSize: 8,
                    formatter: function(params) {
                        return params.value + 'm'
                    }
                }
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
    margin: -1px 0 2px 0;
    font-size: 23px;
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
    height: calc(100% - 33px);
}
</style>