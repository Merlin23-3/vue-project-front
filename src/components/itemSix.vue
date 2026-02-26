<template>
    <div class="item-six-wrapper">
        <div dv-bg class="item-six-container">
            <div class="header">
                <h2>智能分析反馈</h2>
                <!-- 右下角角标 -->
                <div class="corner-mark" @click="openModal">
                    <span class="expand-icon">⤢</span>
                </div>
            </div>

            <!-- 文本内容展示区域 -->
            <div class="content-area">
                <div class="text-content" ref="textContentRef">
                    {{ displayText }}
                </div>
            </div>
        </div>
 

        <teleport to="body">
            <div v-if="showModal" class="modal-overlay" @click.self="closeModal">
                <div class="modal-content">
                    <div class="modal-header">
                        <h3>智能分析反馈 - 放大展示</h3>
                        <button class="close-btn" @click="closeModal">✕</button>
                    </div>
                    <div class="modal-body">
                        <div class="modal-text">{{ displayText }}</div>
                    </div>
                </div>
            </div>
        </teleport>
    </div>
</template>

<script setup>
import { ref, onUnmounted } from 'vue';

// 修正：先对字符串调用 trim()，再传给 ref
const displayText = ref(`
【海洋环境智能分析报告】
时间：2024年3月15日 14:30

1. 中尺度涡旋识别
   - 在黄海中部发现一个气旋式涡旋，中心位于 122.5°E, 36.2°N
   - 涡旋半径约 85km，强度中等
   - 边界清晰度：良好，分割准确率约92%

2. 水文要素预测（未来72小时）
   - 温度：整体上升趋势，表层水温将升高1.2-1.8℃
   - 盐度：受径流影响，近岸盐度略有下降，变化幅度约0.3PSU
   - 流速：黄海暖流增强，最大流速可达0.8m/s

3. 风浪异常预警
   - 未来24小时有冷空气南下，风速将骤增至12-15m/s
   - 伴随波高升至2.5-3.0m，请注意航行安全
   - 与历史台风“梅花”路径对比无直接关联

4. 建议
   - 养殖区关注水温变化，提前做好防寒措施
   - 港口作业注意大风天气，及时调度
   - 继续监测涡旋发展，可能影响渔业资源分布
`.trim());

// 弹窗控制
const showModal = ref(false);

const openModal = () => {
    showModal.value = true;
    document.body.style.overflow = 'hidden';
};

const closeModal = () => {
    showModal.value = false;
    document.body.style.overflow = 'auto';
};

// 键盘ESC关闭弹窗
const handleKeydown = (e) => {
    if (e.key === 'Escape' && showModal.value) {
        closeModal();
    }
};

// 添加键盘监听
window.addEventListener('keydown', handleKeydown);

// 清理事件监听器
onUnmounted(() => {
    window.removeEventListener('keydown', handleKeydown);
});
</script>

<style scoped>
.item-six-wrapper {
    width: 100%;
    height: 100%;
    position: relative;
}

.header h2 {
    color: white;
    margin: 0;
}

.item-six-container {
    height: 100%;
    display: flex;
    flex-direction: column;
    padding: 8px 5px 5px 5px;
    box-sizing: border-box;
    position: relative;
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


.content-area {
    flex: 1;
    min-height: 0;
    width: 100%;
    background: rgba(0, 0, 0, 0.2);
    border-radius: 6px;
    padding: 10px;
    overflow: hidden;
    box-sizing: border-box;
}

.text-content {
    width: 100%;
    height: 100%;
    overflow-y: auto;
    color: #fff;
    font-size: 12px;
    line-height: 1.6;
    white-space: pre-wrap;
    word-break: break-word;
    font-family: 'Courier New', monospace;
    scrollbar-width: thin;
    scrollbar-color: #00f2fe rgba(0, 0, 0, 0.3);
}

.text-content::-webkit-scrollbar {
    width: 6px;
}
.text-content::-webkit-scrollbar-track {
    background: rgba(0, 0, 0, 0.3);
    border-radius: 3px;
}
.text-content::-webkit-scrollbar-thumb {
    background: #00f2fe;
    border-radius: 3px;
}


.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.7);
    backdrop-filter: blur(3px);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 9999;
    animation: fadeIn 0.2s ease;
}

.modal-content {
    width: 80vw;
    height: 80vh;
    background: #0a1a2a;
    border: 2px solid #00f2fe;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    box-shadow: 0 0 30px rgba(0, 242, 254, 0.3);
    animation: slideIn 0.3s ease;
}

.modal-header {
    height: 50px;
    padding: 0 20px;
    background: linear-gradient(90deg, #0a2a3a, #1a3a4a);
    border-bottom: 1px solid #00f2fe;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-shrink: 0;
}

.modal-header h3 {
    color: #00f2fe;
    margin: 0;
    font-size: 18px;
}

.close-btn {
    width: 32px;
    height: 32px;
    background: transparent;
    border: 1px solid #ff6b6b;
    color: #ff6b6b;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s;
}

.close-btn:hover {
    background: #ff6b6b;
    color: #fff;
}

.modal-body {
    flex: 1;
    min-height: 0;
    padding: 20px;
    overflow: auto;
    color: #fff;
    font-size: 14px;
    line-height: 1.8;
    white-space: pre-wrap;
    font-family: 'Courier New', monospace;
}

.modal-text {
    width: 100%;
    height: 100%;
    overflow-y: auto;
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
</style>