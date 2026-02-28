<template>
<div demo-bg>
    <dv-border-box11 title="海洋环境智能分析系统" :title-width="400" :animate="false">
      <div dv-bg>
        <div>
          <!--大容器-->
          <section class="container">
            <!--left-->
            <section class="itemLeft" :class="{ 'sidebar-hidden': !showSidebars }">
                  <section class="k1">
                    <dv-border-box-12 v-if="renderBorder" style="width: 100%; height: 100%;">
                      <div style="width: 100%; height: 100%; padding: 10px;">
                        <ItemPage>
                          <itemFive/>
                        </ItemPage>
                      </div>
                    </dv-border-box-12>
                    <div v-else style="width: 100%; height: 100%; padding: 10px;">
                      <ItemPage>
                        <itemFive/>
                      </ItemPage>
                    </div>
                  </section>
                  <section class="k2">
                    <dv-border-box-12 v-if="renderBorder" style="width: 100%; height: 100%;">
                      <div style="width: 100%; height: 100%; padding: 10px;">
                        <ItemPage>
                          <itemTwo/>
                        </ItemPage>
                      </div>
                    </dv-border-box-12>
                    <div v-else style="width: 100%; height: 100%; padding: 10px;">
                      <ItemPage>
                        <itemTwo/>
                      </ItemPage>
                    </div>
                  </section>
                  <section class="k3">
                    <dv-border-box-12 v-if="renderBorder" style="width: 100%; height: 100%;">
                      <div style="width: 100%; height: 100%; padding: 10px;">
                        <ItemPage>
                          <itemOne/>
                        </ItemPage>
                      </div>
                    </dv-border-box-12>
                    <div v-else style="width: 100%; height: 100%; padding: 10px;">
                      <ItemPage>
                        <itemOne/>
                      </ItemPage>
                    </div>
                  </section>
            </section>

            <!--center-->
            <section class="itemCenter" :class="{ 'center-full': !showSidebars }" @click="toggleSidebars">
              <dv-border-box10 style="width: 100%; height: 100%;">
                <div style="width: 100%; height: 100%; display: flex; align-items: center; justify-content: center; position: relative; cursor: pointer;">
                  <h2>地图</h2>
                  <dv-decoration-12 style="position: absolute; top: 10px; left: 10px; width: 100px; height: 100px;" />
                </div>
              </dv-border-box10>
            </section>

            <!--right-->
            <section class="itemRight" :class="{ 'sidebar-hidden': !showSidebars }">
              <section class="k4">
                <dv-border-box-12 v-if="renderBorder" style="width: 100%; height: 100%;">
                  <div style="width: 100%; height: 100%; padding: 10px;">
                    <ItemPage>
                      <itemThree/>
                    </ItemPage>
                  </div>
                </dv-border-box-12>
                <div v-else style="width: 100%; height: 100%; padding: 10px;">
                  <ItemPage>
                    <itemThree/>
                  </ItemPage>
                </div>
              </section>
              <section class="k5">
                <dv-border-box-12 v-if="renderBorder" style="width: 100%; height: 100%;">
                  <div style="width: 100%; height: 100%; padding: 10px;">
                    <ItemPage>
                      <itemFour/>
                    </ItemPage>
                  </div>
                </dv-border-box-12>
                <div v-else style="width: 100%; height: 100%; padding: 10px;">
                  <ItemPage>
                    <itemFour/>
                  </ItemPage>
                </div>
              </section>
              <section class="k6">
                <dv-border-box-12 v-if="renderBorder" style="width: 100%; height: 100%;">
                  <div style="width: 100%; height: 100%; padding: 10px;">
                    <ItemPage>
                      <itemSix/>
                    </ItemPage>
                  </div>
                </dv-border-box-12>
                <div v-else style="width: 100%; height: 100%; padding: 10px;">
                  <ItemPage>
                    <itemSix/>
                  </ItemPage>
                </div>
              </section>
            </section>
          </section>
        </div>
      </div>
    </dv-border-box11>
  </div>
</template>

<script setup lang="js">
import ItemPage  from '@/components/itemPage.vue';
import itemOne from '@/components/itemOne.vue';
import itemTwo from '@/components/itemTwo.vue';
import itemThree from '@/components/itemThree.vue';
import itemFour from '@/components/itemFour.vue';
import itemFive from '@/components/itemFive.vue';
import itemSix from '@/components/itemSix.vue';
//import mapPage from '@/components/mapPage.vue';
import { inject, ref} from 'vue';
//import dataview from '@jiaminghi/data-view'

let $echarts=inject("echarts")
let $axios=inject("axios")
console.log($echarts,$axios)

// 控制侧边栏显示/隐藏的状态
const showSidebars = ref(false)
// 控制 datav 边框是否渲染
const renderBorder = ref(false)

// 切换侧边栏显示/隐藏
const toggleSidebars = () => {
  if (showSidebars.value) {
    // 隐藏侧边栏
    showSidebars.value = false
    renderBorder.value = false
  } else {
    // 显示侧边栏，先显示容器，等待过渡完成后再渲染边框
    showSidebars.value = true
    // 等待 CSS 过渡完成后再渲染 datav 边框
    setTimeout(() => {
      renderBorder.value = true
    }, 350)
  }
}
</script>

<style lang="less">
// 引入科幻字体
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap');

// 让外层容器占满整个视口
[demo-bg] {
    height: 100vh;
    width: 100vw;
    overflow: hidden;
}

.container{
  min-width: 1200px;
  max-width: 20480px;
  margin: 0 auto;
  padding: 10px;
  height: calc(100vh);
  display: flex;
  box-sizing: border-box;
  align-items: flex-end;
  
  // 左右部分设置比例
  .itemLeft{
    flex: 3;
    height: 97%;
    display: flex;
    flex-direction: column;
    gap: 6px;
    margin-left: 0px;
    padding: 3px;
    min-width: 1px;
    overflow: hidden;
    transition: flex 0.3s ease, opacity 0.3s ease;

    &.sidebar-hidden{
      flex: 0;
      opacity: 0;
      min-width: 0;
      padding: 0;
    }
  }

  .itemCenter{
    flex: 5;
    height: 93%;
    padding: 5px;
    margin: 5px;
    min-width: 0;
    transition: flex 0.3s ease;

    &.center-full{
      flex: 1;
      width: 100%;
    }

    h2{
      height: 48px;
      color: #cd1a;
      line-height: 48px;
      text-align: center;
      font-size: 0.3125rem;
      margin-bottom: 10px;
    }
  }

  .itemRight{
    flex: 3;
    height: 97%;
    display: flex;
    flex-direction: column;
    gap: 6px;
    padding: 3px;
    min-width: 0;
    margin-left: 0;
    overflow: hidden;
    transition: flex 0.3s ease, opacity 0.3s ease;

    &.sidebar-hidden{
      flex: 0;
      opacity: 0;
      min-width: 0;
      padding: 0;
    }
  }
  
  // 左侧三个区块平均分配高度
  .k1, .k2, .k3 {
    flex: 1;
    min-height: 0;
    width: 100%;
    display: flex;
    
    > * {
      width: 100%;
      height: 100%;
    }
  }
  
  // 右侧保持平均分配
  .k4, .k5, .k6 {
    flex: 1;
    min-height: 0;
    width: 100%;
    display: flex;
    
    > * {
      width: 100%;
      height: 100%;
    }
  }
}

// 科幻风格标题样式
.dv-border-box11 {
  :deep(.border-box-title) {
    font-family: 'Orbitron', 'PingFang SC', 'Microsoft YaHei', sans-serif !important;
    font-size: 28px !important;
    font-weight: 700 !important;
    letter-spacing: 6px !important;
    text-transform: uppercase !important;
    background: linear-gradient(135deg, #00f2fe, #4dabf7, #a0e9ff, #00f2fe) !important;
    background-size: 300% 300% !important;
    -webkit-background-clip: text !important;
    -webkit-text-fill-color: transparent !important;
    text-shadow: 0 0 15px rgba(0, 242, 254, 0.8), 0 0 30px rgba(77, 171, 247, 0.6) !important;
    animation: glowShift 4s ease infinite, flicker 3s infinite alternate;
    transform: translateY(2px);
  }
}

// 渐变流动动画
@keyframes glowShift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

// 微光闪烁动画
@keyframes flicker {
  0% { opacity: 0.95; text-shadow: 0 0 15px #00f2fe, 0 0 25px #4dabf7; }
  100% { opacity: 1; text-shadow: 0 0 25px #00f2fe, 0 0 40px #4dabf7, 0 0 60px #a0e9ff; }
}

// 调试样式（如需查看区域边界可取消注释）
//.itemLeft { background: rgba(255,0,0,0.1); }
//.itemCenter { background: rgba(0,255,0,0.1); }
//.itemRight { background: rgba(0,0,255,0.1); }
</style>