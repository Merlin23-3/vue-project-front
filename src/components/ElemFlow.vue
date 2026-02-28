<!-- eslint-disable -->
<template>
    <div class="chart-wrapper">
        <h3>流速分布</h3>
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
               // text: '流速时空分布',
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
        { offset: 0, color: '#90EE90' },  // 薄荷绿
        { offset: 1, color:  '#32CD32' }   // 深绿松石
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
        { offset: 0, color:'#AFEEEE' },  // 深蓝
        { offset: 1, color: '#20B2AA' }   // 近乎黑蓝
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