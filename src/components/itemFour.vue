<template>
    <div class="item-wrapper">
        <div dv-bg class="item-content" style="width: 100%; height: 100%;">
            <div class="chart" id="fourchart"></div>
        </div>
    </div>
</template>
<script setup lang="js">
import { inject, onMounted, onUnmounted } from 'vue';

let $echarts = inject("echarts")
let myChart = null
let resizeObserver = null

const initChart = () => {
    const chartDom = document.getElementById("fourchart")
    if (!chartDom || !chartDom.parentElement) return
    
    const { width, height } = chartDom.parentElement.getBoundingClientRect()
    if (width === 0 || height === 0) return
    
    if (myChart) {
        myChart.dispose()
    }
    
    myChart = $echarts.init(chartDom)
        
        // 定义权重数据
        const weightData = {
            nodes: {
                '温度': 0.95,
                '盐度': 0.85,
                '流速': 0.90,
                '风速': 0.75,
                '波高': 0.70,
                '涡旋': 0.80
            },
            links: {
                '温度-盐度': 0.8,
                '温度-流速': 0.9,
                '盐度-流速': 0.7,
                '风速-波高': 0.95,
                '风速-流速': 0.6,
                '流速-涡旋': 0.85
            }
        }

        // 类别颜色（保留但不直接使用）
        const categories = [
            { name: '水文要素', itemStyle: { color: '#C46B2F' } },
            { name: '动力要素', itemStyle: { color: '#0F5A9E' } },
            { name: '中尺度现象', itemStyle: { color: '#551A8B' } }
        ]

        myChart.setOption({
            title: {
                left: 'center',
                top: 5,
                textStyle: {
                    color: '#fff',
                    fontSize: 14,
                    fontWeight: 'normal'
                },
                subtextStyle: {
                    color: 'rgba(255,255,255,0.7)',
                    fontSize: 12
                }
            },
            series: [{
                type: 'graph',
                layout: 'force',
                categories: categories,
                draggable: true,
                data: [
                    { 
                        name: '温度', 
                        category: 0,
                        value: weightData.nodes['温度'],
                        symbolSize: 40 + weightData.nodes['温度'] * 40,
                        itemStyle: {
                            color: new $echarts.graphic.RadialGradient(0.5, 0.5, 0.5, [
                                { offset: 0, color: 'rgba(196,107,47,1)' },    // 中心深橙，不透明
                                { offset: 1, color: 'rgba(196,107,47,0.2)' }   // 边缘浅橙，半透明
                            ])
                        },
                        label: {
                            show: true,
                            formatter: `温度\n${Math.round(weightData.nodes['温度'] * 100)}%`
                        }
                    },
                    { 
                        name: '盐度', 
                        category: 0,
                        value: weightData.nodes['盐度'],
                        symbolSize: 40 + weightData.nodes['盐度'] * 40,
                        itemStyle: {
                            color: new $echarts.graphic.RadialGradient(0.5, 0.5, 0.5, [
                                { offset: 0, color: 'rgba(15,90,158,1)' },    // 中心深蓝
                                { offset: 1, color: 'rgba(15,90,158,0.2)' }   // 边缘浅蓝
                            ])
                        },
                        label: {
                            show: true,
                            formatter: `盐度\n${Math.round(weightData.nodes['盐度'] * 100)}%`
                        }
                    },
                    { 
                        name: '流速', 
                        category: 1,
                        value: weightData.nodes['流速'],
                        symbolSize: 40 + weightData.nodes['流速'] * 40,
                        itemStyle: {
                            color: new $echarts.graphic.RadialGradient(0.5, 0.5, 0.5, [
                                { offset: 0, color: 'rgba(31,139,31,1)' },    // 中心深绿
                                { offset: 1, color: 'rgba(31,139,31,0.2)' }   // 边缘浅绿
                            ])
                        },
                        label: {
                            show: true,
                            formatter: `流速\n${Math.round(weightData.nodes['流速'] * 100)}%`
                        }
                    },
                    { 
                        name: '风速', 
                        category: 1,
                        value: weightData.nodes['风速'],
                        symbolSize: 40 + weightData.nodes['风速'] * 40,
                        itemStyle: {
                            color: new $echarts.graphic.RadialGradient(0.5, 0.5, 0.5, [
                                { offset: 0, color: 'rgba(201,164,0,1)' },    // 中心深金
                                { offset: 1, color: 'rgba(201,164,0,0.2)' }   // 边缘浅金
                            ])
                        },
                        label: {
                            show: true,
                            formatter: `风速\n${Math.round(weightData.nodes['风速'] * 100)}%`
                        }
                    },
                    { 
                        name: '波高', 
                        category: 1,
                        value: weightData.nodes['波高'],
                        symbolSize: 40 + weightData.nodes['波高'] * 40,
                        itemStyle: {
                            color: new $echarts.graphic.RadialGradient(0.5, 0.5, 0.5, [
                                { offset: 0, color: 'rgba(39,155,143,1)' },   // 中心深青
                                { offset: 1, color: 'rgba(39,155,143,0.2)' }  // 边缘浅青
                            ])
                        },
                        label: {
                            show: true,
                            formatter: `波高\n${Math.round(weightData.nodes['波高'] * 100)}%`
                        }
                    },
                    { 
                        name: '涡旋', 
                        category: 2,
                        value: weightData.nodes['涡旋'],
                        symbolSize: 40 + weightData.nodes['涡旋'] * 40,
                        itemStyle: {
                            color: new $echarts.graphic.RadialGradient(0.5, 0.5, 0.5, [
                                { offset: 0, color: 'rgba(85,26,139,1)' },    // 中心深紫
                                { offset: 1, color: 'rgba(85,26,139,0.2)' }   // 边缘浅紫
                            ])
                        },
                        label: {
                            show: true,
                            formatter: `涡旋\n${Math.round(weightData.nodes['涡旋'] * 100)}%`
                        }
                    }
                ],
                links: [
                    { 
                        source: '温度', 
                        target: '盐度',
                        value: weightData.links['温度-盐度'],
                        lineStyle: {
                            width: weightData.links['温度-盐度'] * 5,
                            color: '#C46B2F',
                            curveness: 0.2
                        },
                        label: {
                            show: true,
                            formatter: `${Math.round(weightData.links['温度-盐度'] * 100)}%`,
                            fontSize: 10,
                            color: '#ffd700'
                        }
                    },
                    { 
                        source: '温度', 
                        target: '流速',
                        value: weightData.links['温度-流速'],
                        lineStyle: {
                            width: weightData.links['温度-流速'] * 5,
                            color: '#C46B2F',
                            curveness: 0.2
                        },
                        label: {
                            show: true,
                            formatter: `${Math.round(weightData.links['温度-流速'] * 100)}%`,
                            fontSize: 10,
                            color: '#ffd700'
                        }
                    },
                    { 
                        source: '盐度', 
                        target: '流速',
                        value: weightData.links['盐度-流速'],
                        lineStyle: {
                            width: weightData.links['盐度-流速'] * 5,
                            color: '#0F5A9E',
                            curveness: 0.2
                        },
                        label: {
                            show: true,
                            formatter: `${Math.round(weightData.links['盐度-流速'] * 100)}%`,
                            fontSize: 10,
                            color: '#ffd700'
                        }
                    },
                    { 
                        source: '风速', 
                        target: '波高',
                        value: weightData.links['风速-波高'],
                        lineStyle: {
                            width: weightData.links['风速-波高'] * 5,
                            color: '#C9A400',
                            curveness: 0.2
                        },
                        label: {
                            show: true,
                            formatter: `${Math.round(weightData.links['风速-波高'] * 100)}%`,
                            fontSize: 10,
                            color: '#ffd700'
                        }
                    },
                    { 
                        source: '风速', 
                        target: '流速',
                        value: weightData.links['风速-流速'],
                        lineStyle: {
                            width: weightData.links['风速-流速'] * 5,
                            color: '#C9A400',
                            curveness: 0.2
                        },
                        label: {
                            show: true,
                            formatter: `${Math.round(weightData.links['风速-流速'] * 100)}%`,
                            fontSize: 10,
                            color: '#ffd700'
                        }
                    },
                    { 
                        source: '流速', 
                        target: '涡旋',
                        value: weightData.links['流速-涡旋'],
                        lineStyle: {
                            width: weightData.links['流速-涡旋'] * 5,
                            color: '#1F8B1F',
                            curveness: 0.2
                        },
                        label: {
                            show: true,
                            formatter: `${Math.round(weightData.links['流速-涡旋'] * 100)}%`,
                            fontSize: 10,
                            color: '#ffd700'
                        }
                    }
                ],
                force: {
                    repulsion: 500,
                    edgeLength: 200,
                    gravity: 0.1,
                    friction: 0.1
                },
                roam: true,
                focusNodeAdjacency: {
                    show: true
                },
                edgeSymbol: ['none', 'arrow'],
                edgeSymbolSize: [0, 8],
                lineStyle: {
                    color: 'source',
                    curveness: 0.2,
                    width: 2,
                    opacity: 0.7
                },
                label: {
                    show: true,
                    position: 'inside',
                    offset: [0, 0],
                    color: '#fff',
                    fontSize: 12,
                    fontFamily: 'PingFang SC, Microsoft YaHei, WenQuanYi Micro Hei, sans-serif',
                    lineHeight: 15,
                    fontWeight: 'bold',
                    align: 'center',
                    verticalAlign: 'middle'
                },
                emphasis: {
                    focus: 'adjacency',
                    lineStyle: {
                        width: 4
                    }
                }
            }],
            toolbox: {
                show: true,
                feature: {
                    saveAsImage: { show: true },
                    restore: { show: true },
                    magicType: { show: true, type: ['force', 'circular'] }
                },
                right: 10,
                top: 30,
                iconStyle: {
                    borderColor: '#fff'
                }
            },
            tooltip: {
                trigger: 'item',
                formatter: function(params) {
                    if (params.dataType === 'node') {
                        return `${params.name}<br/>权重: ${Math.round(params.value * 100)}%<br/>类别: ${params.data.category === 0 ? '水文要素' : params.data.category === 1 ? '动力要素' : '中尺度现象'}`
                    } else {
                        return `${params.data.source} → ${params.data.target}<br/>关系强度: ${Math.round(params.data.value * 100)}%`
                    }
                }
            }
        })

        window.addEventListener('resize', () => {
            if (myChart) myChart.resize()
        })
}

onMounted(() => {
    setTimeout(() => {
        const chartDom = document.getElementById("fourchart")
        if (chartDom && chartDom.parentElement) {
            const { width, height } = chartDom.parentElement.getBoundingClientRect()
            if (width > 0 && height > 0) {
                initChart()
            }
        }
        
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
    }, 100)
})

onUnmounted(() => {
    if (resizeObserver) {
        resizeObserver.disconnect()
    }
    if (myChart) {
        myChart.dispose()
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