<template>
    <div class="item-wrapper">
            <div dv-bg class="item-content" style="width: 100%; height: 100%;">
                <div class="chart" id="onechart"></div>
            </div>
    </div>
</template>

<script setup lang="js">
import { inject, onMounted, onUnmounted } from 'vue';

let $echarts=inject("echarts")
let mychart = null
let resizeObserver = null

const initChart = () => {
   const chartDom = document.getElementById("onechart")
   if (!chartDom || !chartDom.parentElement) return
   
   const { width, height } = chartDom.parentElement.getBoundingClientRect()
   if (width === 0 || height === 0) return
   
   if (mychart) {
       mychart.dispose()
   }
   
   mychart= $echarts.init(chartDom)
   mychart.setOption({
    title: {
        // text: '涡旋数量分布',
        text:"实时数据展示",
        left: 'center',
        textStyle: { color: '#fff', fontSize: 12 }
    },

    grid:{
        top:"15%",
        left:"1%",
        right:"6%",
        bottom:"3%",
        containLabel:true
    },
 
  tooltip: {
    trigger: 'axis',
    axisPointer: {
      type: 'shadow'
    }
   },
    xAxis:{
        type:"value",
        boundaryGap: [0, 0.01]       
    },
    yAxis:{
        type:"category",
        data: ['项目一', '项目二', '项目三', '项目四', '项目五', '项目六']

    },
    series: [
{
name: '2011',
type: 'bar',
itemStyle: {
normal: {
    barBorderRadius:[0,20,20,0],
color: new $echarts.graphic.LinearGradient(0, 0, 1, 0, [
    { offset: 0, color: '#00f2fe' },      // 亮青蓝色
    { offset: 0.5, color: '#00ffea' },    // 亮青色
    { offset: 1, color: '#00ffaa' }       // 亮青绿色
])
}
},
data: [1823, 2489, 2034, 10970, 13144, 43230]
},
{
name: '2012',
type: 'bar',
itemStyle: {
normal: {
    barBorderRadius:[0,20,20,0],
color: new $echarts.graphic.LinearGradient(0, 0, 1, 0, [
    { offset: 0, color: '#ff6b6b' },      // 亮橙红色
    { offset: 0.5, color: '#ffa726' },    // 亮橙色
    { offset: 1, color: '#ffeb3b' }       // 亮黄色
])
}
},
data: [9325, 2438, 3100, 12594, 14141, 38807]
}
]

   })
}

onMounted(() => {
    setTimeout(() => {
        const chartDom = document.getElementById("onechart")
        if (chartDom && chartDom.parentElement) {
            const { width, height } = chartDom.parentElement.getBoundingClientRect()
            if (width > 0 && height > 0) {
                initChart()
            }
        }
        
        // 使用 ResizeObserver 监听容器尺寸变化
        if (chartDom && chartDom.parentElement) {
            resizeObserver = new ResizeObserver((entries) => {
                for (const entry of entries) {
                    const { width, height } = entry.contentRect
                    if (width > 0 && height > 0) {
                        if (!mychart) {
                            initChart()
                        } else {
                            mychart.resize()
                        }
                    }
                }
            })
            resizeObserver.observe(chartDom.parentElement)
        }
    }, 100)
})

onUnmounted(() => {
    if (resizeObserver) {
        resizeObserver.disconnect()
    }
    if (mychart) {
        mychart.dispose()
    }
})
</script>

<style>
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
}

.chart {
    flex: 1;
    min-height: 0;
    width: 100%;
}

h2 {
    height: 48px;
    color: #cd1a;
    line-height: 48px;
    text-align: center;
    font-size: 0.3125rem;
    margin: 0;
    flex-shrink: 0;
}
</style> 