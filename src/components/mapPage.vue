<!-- <template>
    <div class="mapbk" id="mapbk">
    <dv-border-box8 :reverse="true">
      <div dv-bg>
        <div class="map" id="map"></div>
      </div>
      
    </dv-border-box8>
    </div>
</template>    

<script setup lang="js">
import { onMounted, inject, ref } from 'vue'
import { mapData } from "../assets/mapData.JS"

let $echarts = inject("echarts")
const chartInstance = ref(null)

onMounted(() => {
    try {
        console.log('地图数据:', mapData)
        console.log('地图特征数量:', mapData.features?.length || 0)
        
        
        $echarts.registerMap("HK", mapData)  
        
        
        const registeredMap = $echarts.getMap("HK")
        console.log('地图注册成功:', registeredMap ? '是' : '否')
        
        
        const mapElement = document.getElementById("map")
        if (!mapElement) {
            console.error('找不到地图容器')
            return
        }
        
    
        chartInstance.value = $echarts.init(mapElement)
        
     
        const chartData = [
            { name: '中西区', value: 20057.34 },
            { name: '湾仔区', value: 15477.48 },    
            { name: '东区', value: 31686.1 },        
            { name: '南区', value: 6992.6 },         
            { name: '油尖旺区', value: 44045.49 }, 
            { name: '深水埗区', value: 40689.64 }, 
            { name: '九龙城区', value: 37659.78 }, 
            { name: '黄大仙区', value: 45180.97 }, 
            { name: '观塘区', value: 55204.26 },   
            { name: '葵青区', value: 21900.9 },     
            { name: '荃湾区', value: 4918.26 },    
            { name: '屯门区', value: 5881.84 },    
            { name: '元朗区', value: 4178.01 },    
            { name: '北区', value: 2227.92 },        
            { name: '大埔区', value: 2180.98 },    
            { name: '沙田区', value: 9172.94 },      
            { name: '西贡区', value: 3368 },         
            { name: '离岛区', value: 806.98 }        
        ]
        

        const option = { 
           
            tooltip: {
                trigger: 'item',
                formatter: '{b}<br/>{c} (人/平方公里)'
            },
            toolbox: {
                show: true,
                orient: 'vertical',
                left: 'right',
                top: 'center',
                feature: {
                    dataView: { readOnly: false },
                    restore: {},
                    saveAsImage: {}
                }
            },
            visualMap: {
                min: 800,
                max: 50000,
                text: ['高', '低'],
                realtime: false,
                calculable: true,
                inRange: {
                    color: ['lightskyblue', 'yellow', 'orangered']
                }
            },
            series: [
                {
                    name: '香港18区人口密度',
                    type: 'map',
                    map: 'HK',  
                    label: {
                        show: true,
                        fontSize: 10
                    },
                    emphasis: {
                        label: {
                            show: true,
                            fontSize: 12,
                            fontWeight: 'bold'
                        },
                        itemStyle: {
                            areaColor: '#389BB7'
                        }
                    },
                    data: chartData,
                    
                }
            ]
        }
        
        chartInstance.value.setOption(option)
        window.addEventListener('resize', () => {
            if (chartInstance.value) {
                chartInstance.value.resize()
            }
        })
        
    } catch (error) {
        console.error('地图初始化失败:', error)
    }
})

// 组件卸载时销毁图表
import { onUnmounted } from 'vue'
onUnmounted(() => {
    if (chartInstance.value) {
        chartInstance.value.dispose()
    }
    window.removeEventListener('resize', () => {})
})
</script>

<style scoped>
.map {
    width: 100%;
    height: 700px;
    min-height: 400px; }
.mapbk{
  width: 100%;
  height: 800px;
}    
</style> -->