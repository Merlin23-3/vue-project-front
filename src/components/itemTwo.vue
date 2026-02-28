<template>
    <div class="item-two-wrapper">
        <div dv-bg class="item-two-container" style="width: 100%; height: 100%;">
            <div class="header">
                <!-- 右下角角标 - 透明背景 -->
                <div class="corner-mark" @click="openModal">
                    <span class="expand-icon">⤢</span>
                </div>
            </div>
            
            <!-- 图表容器 - 扩大占比 -->
            <div class="chart-container">
                <component 
                    :is="currentChart" 
                    :chart-id="`chart-${currentChartId}`"
                    :key="currentChartId"
                />
            </div>

            <!-- Decoration2 装饰 -->
            <dv-decoration-2 style="width: 100%; height: 5px;" />

            <!-- 底部切换按钮 - 减小高度 -->
            <div class="switch-wrapper">
                <ChartSwitch 
                    :active-id="currentChartId"
                    @switch="handleSwitch"
                />
            </div>
        </div>

        <!-- 弹窗组件（配色已调整） -->
        <teleport to="body">
            <div v-if="showModal" class="modal-overlay" @click.self="closeModal">
                <div class="modal-content" :class="{ 'modal-enter': showModal }">
                    <div class="modal-header">
                        <button class="close-btn" @click="closeModal">✕</button>
                    </div>
                    <div class="modal-body">
                        <!-- 放大版本的图表 -->
                        <component 
                            :is="currentChart" 
                            :chart-id="`modal-chart-${currentChartId}`"
                            :key="`modal-${currentChartId}`"
                            class="modal-chart"
                        />
                    </div>
                    <div class="modal-footer">
                        <ChartSwitch 
                            :active-id="currentChartId"
                            @switch="handleModalSwitch"
                        />
                    </div>
                </div>
            </div>
        </teleport>
    </div>
</template>

<script setup>
import { ref, shallowRef, markRaw } from 'vue';
import ElemTemp from './ElemTemp.vue'
import ElemSalinity from './ElemSalinity.vue'
import ElemFlow from './ElemFlow.vue'
import ElemWind from './ElemWind.vue'
import ElemWave from './ElemWave.vue'
import ElemEddy from './ElemEddy.vue'
import ChartSwitch from './ChartSwitch.vue'


const currentChartId = ref('temp')
const currentChart = shallowRef(markRaw(ElemTemp))
const showModal = ref(false)
const chartMap = {
    temp: { component: ElemTemp, name: '温度' },
    salinity: { component: ElemSalinity, name: '盐度' },
    flow: { component: ElemFlow, name: '流速' },
    wind: { component: ElemWind, name: '风速' },
    wave: { component: ElemWave, name: '波高' },
    eddy: { component: ElemEddy, name: '涡旋' }
}

// 切换图表（主视图）
const handleSwitch = (chartId) => {
    currentChartId.value = chartId
    currentChart.value = markRaw(chartMap[chartId].component)
}

// 切换图表（弹窗视图）
const handleModalSwitch = (chartId) => {
    currentChartId.value = chartId
    currentChart.value = markRaw(chartMap[chartId].component)
}

// 打开弹窗
const openModal = () => {
    showModal.value = true
    document.body.style.overflow = 'hidden'
}

// 关闭弹窗
const closeModal = () => {
    showModal.value = false
    document.body.style.overflow = 'auto'
}

// 键盘ESC关闭弹窗
const handleKeydown = (e) => {
    if (e.key === 'Escape' && showModal.value) {
        closeModal()
    }
}

window.addEventListener('keydown', handleKeydown)
</script>

<style>
.item-two-wrapper {
    width: 100%;
    height: 100%;
    position: relative;
}


.item-two-container {
    height: 100%;
    display: flex;
    flex-direction: column;
    padding: 8px 5px 5px 5px;
    position: relative;
    box-sizing: border-box;
}

.header {
    position: relative;
    height: 24px;
    flex-shrink: 0;
    margin-bottom: 2px;
}

h2 {
    color: #cd1a;
    line-height: 24px;
    text-align: center;
    font-size: 14px;
    margin: 0;
}

/* 角标样式 - 透明背景 */
.corner-mark {
    position: absolute;
    bottom: -2px;
    right: 2px;
    width: 22px;
    height: 22px;
    background: transparent;
    border: none;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.2s;
    z-index: 10;
    opacity: 0.6;
}

.corner-mark:hover {
    opacity: 1;
    transform: scale(1.1);
    background: rgba(0, 242, 254, 0.1);
    border-radius: 4px;
}

.expand-icon {
    color: #00f2fe;
    font-size: 16px;
    font-weight: bold;
}

/* 图表容器 - 扩大占比 */
.chart-container {
    flex: 1;
    min-height: 0;
    width: 100%;
    margin: 0 0 4px 0;
}

/* 切换按钮 - 减小高度 */
.switch-wrapper {
    height: 34px;
    flex-shrink: 0;
    margin-top: 0;
}

/* 弹窗样式 - 新配色 */
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.7);
    backdrop-filter: blur(10px);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 9999;
    animation: fadeIn 0.2s ease;
}

.modal-content {
    width: 50vw;
    height: 50vh;
    background: #0A1F5C;        /* 深蓝色背景 */
    border: 2px solid #00F7FF;   /* 亮蓝色边框 */
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    box-shadow: 0 0 30px rgba(0, 247, 255, 0.3);
    animation: slideIn 0.3s ease;
}

.modal-header {
    height: 50px;
    padding: 0 20px;
    background: #0047AB;        /* 中蓝色头部 */
    border-bottom: 1px solid #00F7FF;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    flex-shrink: 0;
}

.modal-footer {
    height: 60px;
    padding: 10px;
    border-top: 1px solid #00F7FF;
    background: #0047AB;
    flex-shrink: 0;
}

.close-btn {
    width: 32px;
    height: 32px;
    background: transparent;
    border: 1px solid #00F7FF;
    color: #00F7FF;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s;
}

.close-btn:hover {
    background: #00F7FF;
    color: #0A1F5C;
}

.modal-body {
    flex: 1;
    min-height: 0;
    padding: 15px;
    height: calc(100% - 110px);
    box-sizing: border-box;
}

.modal-chart {
    width: 100%;
    height: 100%;
    min-height: 0;
}

/* 动画 */
@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

@keyframes slideIn {
    from {
        transform: scale(0.9);
        opacity: 0;
    }
    to {
        transform: scale(1);
        opacity: 1;
    }
}

/* 确保图表占满容器 - 调整比例让图表更大 */
:deep(.chart-wrapper) {
    height: 100%;
    width: 100%;
}

:deep(.chart-wrapper .chart) {
    height: calc(100% - 18px) !important;
    width: 100%;
}

:deep(.chart-wrapper h3) {
    height: 18px;
    line-height: 18px;
    margin: 0;
    font-size: 12px;
}

:deep(.switch-buttons) {
    padding: 3px;
    gap: 3px;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

:deep(.switch-btn) {
    padding: 2px 5px;
    font-size: 11px;
}

/* 弹窗内的图表样式调整 - 增强版 */
.modal-body :deep(.chart-wrapper) {
    height: 100%;
    width: 100%;
    background: rgba(0, 0, 0, 0.2);
    border-radius: 8px;
    padding: 10px;
    box-sizing: border-box;
}

.modal-body :deep(.chart-wrapper h3) {
    font-size: 16px;
    height: 24px;
    line-height: 24px;
    margin-bottom: 5px;
    color: #00f2fe;
}

.modal-body :deep(.chart-wrapper .chart) {
    height: calc(100% - 29px) !important;
    width: 100%;
    min-height: 0;
}

/* 确保ECharts canvas填满容器 */
.modal-body :deep(canvas) {
    width: 100% !important;
    height: 100% !important;
} 
</style>