<!-- eslint-disable -->
<template>
    <div class="chart-wrapper">
        <h3>风速分布</h3>
        <div class="chart" :id="chartId"></div>
    </div>
</template>

<script setup>
import { inject, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
    chartId: {
        type: String,
        default: 'wind-chart'
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
              //  text: '风速季节变化',
                left: 'center',
                textStyle: { color: '#fff', fontSize: 12 }
            },
            grid: {
                top: '18%',
                left: '12%',
                right: '8%',
                bottom: '12%',
                containLabel: true
            },
            tooltip: { 
                trigger: 'axis',
                axisPointer: { type: 'shadow' }
            },
            legend: {
                textStyle: { color: '#fff' },
                top: 25,
                itemWidth: 12,
                itemHeight: 8
            },
            xAxis: {
                type: 'category',
                data: ['春季', '夏季', '秋季', '冬季'],
                axisLabel: { 
                    color: '#fff',
                    fontSize: 11
                },
                axisLine: { lineStyle: { color: '#00f2fe' } }
            },
            yAxis: {
                type: 'value',
                name: '风速(m/s)',
                nameTextStyle: { color: '#fff', fontSize: 10 },
                axisLabel: { color: '#fff', fontSize: 9 },
                splitLine: { lineStyle: { color: 'rgba(255,255,255,0.1)' } }
            },
            series: [{
                name: '渤海',
                type: 'bar',
                data: [5.8, 4.5, 6.2, 8.5],
                itemStyle: { color: '#4dabf7' },
                barWidth: '15%'
            }, {
                name: '黄海北部',
                type: 'bar',
                data: [6.2, 4.8, 6.8, 9.2],
                itemStyle: { color: '#3bc9db' },
                barWidth: '15%'
            }, {
                name: '黄海南部',
                type: 'bar',
                data: [6.5, 5.2, 7.2, 9.8],
                itemStyle: { color: '#69db7e' },
                barWidth: '15%'
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

// ResizeObserver 监听容器尺寸变化
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
.chart-wrapper {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
}

h3 {
    color: #fff;
    text-align: center;
    margin: 2px 0;
    font-size: 14px;
    flex-shrink: 0;
}

.chart {
    flex: 1;
    width: 100%;
    min-height: 0;
}
</style>