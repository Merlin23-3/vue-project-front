<!-- eslint-disable -->
<template>
    <div class="chart-wrapper">
        <h3 class="artistic-title">温度分布</h3>
        <div class="chart" :id="chartId"></div>
    </div>
</template>

<script setup>
import { inject, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
    chartId: {
        type: String,
        default: 'temp-chart'
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
                left: '8%',       // 左侧恢复较小值
                right: '15%',      // 右侧增加留白给图例
                bottom: '12%',
                containLabel: true
            },
            tooltip: { 
                trigger: 'axis',
                axisPointer: { type: 'shadow' },
                formatter: function(params) {
                    let res = params[0].name + '<br/>'
                    for (let i = 0; i < params.length; i++) {
                        res += params[i].marker + ' ' + params[i].seriesName + ': ' + params[i].value + '℃<br/>'
                    }
                    return res
                }
            },
            legend: {
                textStyle: { color: '#fff' },
                top: 'center',        // 垂直居中
                right: '0',            // 靠右对齐
                orient: 'vertical',    // 纵向排布
                itemWidth: 12,
                itemHeight: 8,
                itemGap: 15,          // 图例项间距
                borderColor: 'rgba(255,255,255,0.2)',
                borderWidth: 0,
                backgroundColor: 'transparent'
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
                axisLine: { lineStyle: { color: '#00f2fe' } },
                axisTick: { show: false }
            },
            yAxis: {
                type: 'value',
                name: '温度(℃)',
                nameTextStyle: { color: '#fff', fontSize: 10 },
                axisLabel: { color: '#fff', fontSize: 9 },
                splitLine: { lineStyle: { color: 'rgba(255,255,255,0.1)' } },
                nameLocation: 'middle',
                nameGap: 30
            },
            series: [{
                name: '表层温度',
                type: 'line',
                data: [4.2, 3.8, 5.1, 8.3, 12.5, 16.8, 20.3, 22.1, 19.5, 15.2, 10.1, 6.2],
                lineStyle: {
                    width: 3,
                    color: new $echarts.graphic.LinearGradient(0, 1, 0, 0, [
                        { offset: 0, color: '#C69BFA' },  // 浅紫
                        { offset: 1, color: '#552583' }   // 深紫
                    ])
                }
            }, {
                name: '底层温度',
                type: 'line',
                data: [3.8, 3.5, 4.2, 6.1, 9.3, 12.5, 15.2, 16.8, 15.1, 12.3, 8.5, 5.1],
                lineStyle: {
                    width: 3,
                    color: new $echarts.graphic.LinearGradient(0, 1, 0, 0, [
                        { offset: 0, color: '#FFE68F' },  // 浅金
                        { offset: 1, color: '#FDB927' }   // 深金
                    ])
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
/* 引入演示秋鸿楷字体 */
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

/* 艺术字体标题 - 与实时内容展示一致 */
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
    height: calc(100% - 28px);  /* 减去标题高度 */
}
</style>