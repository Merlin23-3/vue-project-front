<template>
    <div class="item-wrapper">
        <div dv-bg class="item-content" style="width: 100%; height: 100%;">
            <!-- 添加标题 -->
            <div class="header">
                <h2 class="artistic-title">要素对比</h2>
            </div>
            <div ref="chartRef" class="chart"></div>
        </div>
    </div>
</template>

<script setup lang="js">
import { ref, onMounted, onUnmounted } from 'vue';
import * as echarts from 'echarts';

let chartInstance = null
const chartRef = ref(null)

// 要素名称
const categories = ['温度', '盐度', '流速', '风速', '波高', '涡旋'];

// 当前数据
const currentData = [22.5, 32.8, 1.25, 8.5, 2.3, 18];
// 预测数据
const predictData = [24.8, 33.2, 1.45, 12.5, 3.1, 22];

// 计算每个要素的当前数据占比（基于当前+预测的总和）
const currentPercent = currentData.map((value, index) => {
    const total = value + predictData[index];
    return Number(((value / total) * 100).toFixed(1));
});

// 计算每个要素的预测数据占比（基于当前+预测的总和）
const predictPercent = predictData.map((value, index) => {
    const total = value + currentData[index];
    return Number(((value / total) * 100).toFixed(1));
});

const initChart = () => {
    if (!chartRef.value) return
    
    // 创建ECharts实例
    chartInstance = echarts.init(chartRef.value)
    
    const option = {
        title: {
            left: 'center',
            textStyle: { color: '#fff', fontSize: 12 }
        },
        // 图例 - 只保留当前数据和预测数据
        legend: {
            data: ['当前数据', '预测数据'],
            textStyle: { color: '#fff' },
            top: 20,
            itemWidth: 12,
            itemHeight: 8,
            orient: 'horizontal',
            left: 'center'
        },
        // 网格配置
        grid: {
            left: '15%',
            right: '10%',
            top: '20%',
            bottom: '5%',
            containLabel: true
        },
        // 工具提示
        tooltip: {
            trigger: 'item',
            formatter: function(params) {
                const index = params.dataIndex;
                const category = categories[index];
                const currentVal = currentData[index].toFixed(1);
                const predictVal = predictData[index].toFixed(1);
                const currentPct = currentPercent[index];
                const predictPct = predictPercent[index];
                const unit = index === 0 ? '℃' : 
                            index === 1 ? 'PSU' : 
                            index === 2 ? 'm/s' : 
                            index === 3 ? 'm/s' : 
                            index === 4 ? 'm' : '个';
                
                if (params.seriesName === '当前数据') {
                    return `${category}<br/>当前: ${currentVal}${unit} (${currentPct}%)`;
                } else {
                    return `${category}<br/>预测: ${predictVal}${unit} (${predictPct}%)`;
                }
            }
        },
        // X轴（数值轴）
        xAxis: {
            type: 'value',
            min: 0,
            max: 100,
            axisLabel: {
                color: '#fff',
                formatter: '{value}%'
            },
            splitLine: {
                show: true,
                lineStyle: {
                    color: 'rgba(255,255,255,0.1)'
                }
            },
            axisLine: { lineStyle: { color: '#00f2fe' } }
        },
        // Y轴（分类轴）
        yAxis: {
            type: 'category',
            data: categories,
            axisLabel: { 
                color: '#fff',
                fontSize: 12,
                fontWeight: 'bold'
            },
            axisLine: { show: false },
            axisTick: { show: false },
            splitLine: { show: false }
        },
        // 系列数据：堆叠条形图
        series: [
            {
                name: '当前数据',
                type: 'bar',
                stack: 'total',
                data: currentPercent,
                barWidth: 20,
                itemStyle: {
                    color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
                        { offset: 0, color: '#00f2fe' },
                        { offset: 1, color: '#4dabf7' }
                    ]),
                    borderRadius: [4, 0, 0, 4]
                },
                label: {
                    show: true,
                    position: 'insideLeft',
                    color: '#fff',
                    fontSize: 11,
                    fontWeight: 'bold',
                    formatter: function(params) {
                        return params.value > 5 ? `${params.value}%` : '';
                    }
                }
            },
            {
                name: '预测数据',
                type: 'bar',
                stack: 'total',
                data: predictPercent,
                itemStyle: {
                    color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
                        { offset: 0, color: '#ff6b6b' },
                        { offset: 1, color: '#ffa726' }
                    ]),
                    borderRadius: [0, 4, 4, 0]
                },
                label: {
                    show: true,
                    position: 'insideRight',
                    color: '#fff',
                    fontSize: 11,
                    fontWeight: 'bold',
                    formatter: function(params) {
                        return params.value > 5 ? `${params.value}%` : '';
                    }
                }
            }
        ]
    }

    // 设置配置项并渲染
    chartInstance.setOption(option)
    
    // 自适应窗口大小
    window.addEventListener('resize', handleResize)
}

const handleResize = () => {
    if (chartInstance) {
        chartInstance.resize()
    }
}

onMounted(() => {
    setTimeout(() => {
        if (chartRef.value) {
            initChart()
        }
    }, 100)
})

onUnmounted(() => {
    window.removeEventListener('resize', handleResize)
    if (chartInstance) {
        chartInstance.dispose()
        chartInstance = null
    }
})
</script>

<style>
/* 引入演示秋鸿楷字体 */
@font-face {
    font-family: '演示秋鸿楷';
    src: url('/src/assets/fonts/演示秋鸿楷.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
    font-display: swap;
}

.item-wrapper {
    width: 100%;
    height: 100%;
}

.item-wrapper :deep(.dv-border-box6) {
    width: 100%;
    height: 100%;
}

.item-wrapper :deep(.dv-border-box6 > div) {
    width: 100%;
    height: 100%;
}

.item-content {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    padding: 0;
    box-sizing: border-box;
}

/* 标题区域 */
.header {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 40px;
    padding: 0 10px;
    flex-shrink: 0;
}

/* 标题艺术字体样式 - 秋鸿楷 */
.artistic-title {
    color: white;
    margin: 0;
    font-size: 24px;
    font-weight: 500;
    font-family: '演示秋鸿楷', '华文楷体', 'KaiTi', '楷体', 'PingFang SC', 'Microsoft YaHei', serif;
    line-height: 40px;
    letter-spacing: 2px;
    text-shadow: 0 2px 8px rgba(0, 242, 254, 0.4);
    transform: scaleY(1.05);
    display: inline-block;
}

.chart {
    flex: 1;
    min-height: 0;
    width: 100%;
}
</style>