<template>
    <div>
        <div dv-bg class="item-five-container">
            <!-- 标题区域（上） -->
            <div class="header">
                <h2>安全预警</h2>
            </div>

            <!-- 主内容区：上下分割 1:6 -->
            <div class="main-content">
                <!-- 上容器：横向颜色示例 -->
                <div class="legend-panel-horizontal">
                    <div class="legend-item">
                        <span class="color-box red"></span>
                        <span class="legend-text">红色预警</span>
                    </div>
                    <div class="legend-item">
                        <span class="color-box yellow"></span>
                        <span class="legend-text">黄色预警</span>
                    </div>
                    <div class="legend-item">
                        <span class="color-box green"></span>
                        <span class="legend-text">一切正常</span>
                    </div>
                </div>

                <!-- 下容器：滚动字幕 -->
                <div class="message-panel" ref="messagePanelRef">
                    <div class="message-list" :style="{ transform: scrollTransform }">
                        <div
                            v-for="(msg, index) in messages"
                            :key="index"
                            class="message-item"
                            :class="{
                                'current': index === currentIndex,
                                'red-msg': msg.type === 'red',
                                'yellow-msg': msg.type === 'yellow',
                                'green-msg': msg.type === 'green'
                            }"
                        >
                            {{ msg.text }}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed, nextTick } from 'vue';

// 模拟消息数据
const messages = ref([
    { text: '红色预警：台风“鲇鱼”即将登陆', type: 'red' },
    { text: '黄色预警：海浪高度超过2.5米', type: 'yellow' },
    { text: '一切正常：当前海域无异常', type: 'green' },
    { text: '黄色预警：风速超过12m/s', type: 'yellow' },
    { text: '红色预警：巨浪红色警报', type: 'red' },
    { text: '一切正常：所有监测指标正常', type: 'green' }
]);

const currentIndex = ref(0);
let timer = null;

// 消息项固定高度（px）
const itemHeight = 40;
// 滚动容器高度（动态获取）
const panelHeight = ref(200); // 默认值，稍后更新

// 获取滚动容器DOM
const messagePanelRef = ref(null);

// 计算滚动偏移：使当前高亮项始终在容器中央
const scrollTransform = computed(() => {
    // 中央位置偏移 = (panelHeight/2 - itemHeight/2) - itemHeight * currentIndex
    const offset = (panelHeight.value / 2 - itemHeight / 2) - itemHeight * currentIndex.value;
    return `translateY(${offset}px)`;
});

// 更新滚动容器高度
const updatePanelHeight = () => {
    if (messagePanelRef.value) {
        panelHeight.value = messagePanelRef.value.clientHeight;
    }
};

// 滚动切换
const startRolling = () => {
    timer = setInterval(() => {
        currentIndex.value = (currentIndex.value + 1) % messages.value.length;
    }, 3000);
};

onMounted(() => {
    // 初始获取高度
    nextTick(() => {
        updatePanelHeight();
    });
    // 监听窗口大小变化，重新计算高度
    window.addEventListener('resize', updatePanelHeight);
    startRolling();
});

onUnmounted(() => {
    if (timer) clearInterval(timer);
    window.removeEventListener('resize', updatePanelHeight);
});
</script>

<style scoped>
.item-five-container {
    height: 100%;
    display: flex;
    flex-direction: column;
    padding: 0;
    box-sizing: border-box;
    background: transparent;
}

.header {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 40px;
    padding: 0 10px;
    flex-shrink: 0;
}

.header h2 {
    color: white;
    margin: 0;
    font-size: 20px;
    font-weight: 500;
    font-family: 'PingFang SC', 'Microsoft YaHei', 'Helvetica Neue', Arial, sans-serif;
    line-height: 40px;
    letter-spacing: 1px;
    text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

/* 主内容区：上下分割 1:6 */
.main-content {
    flex: 1;
    min-height: 0;
    width: 100%;
    display: flex;
    flex-direction: column;
    padding: 8px;
    box-sizing: border-box;
    gap: 8px;
}

/* 上容器：横向颜色示例 */
.legend-panel-horizontal {
    flex: 1; /* 占1份 */
    min-height: 0;
    background: transparent;
    border-radius: 8px;
    padding: 8px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
    gap: 20px;
}

.legend-item {
    display: flex;
    align-items: center;
    gap: 10px;
}

.color-box {
    width: 24px;
    height: 24px;
    border-radius: 4px;
}

.color-box.red {
    background-color: #ff4d4d;
    box-shadow: 0 0 10px #ff4d4d;
}
.color-box.yellow {
    background-color: #ffd93e;
    box-shadow: 0 0 10px #ffd93e;
}
.color-box.green {
    background-color: #4caf50;
    box-shadow: 0 0 10px #4caf50;
}

.legend-text {
    color: white;
    font-size: 16px;
    font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
}

/* 下容器：滚动消息 */
.message-panel {
    flex: 6; /* 占6份 */
    min-height: 0;
    background: transparent;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    padding: 0 10px; /* 左右留白 */
    box-sizing: border-box;
}

/* 滚动列表容器 */
.message-list {
    flex: none;
    width: 100%;
    transition: transform 0.5s ease;
    will-change: transform;
    box-sizing: border-box;
}

/* 每条消息的样式：固定高度，单行省略 */
.message-item {
    height: 40px;
    line-height: 40px;
    text-align: center;
    font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
    opacity: 0.5;
    font-size: 16px;
    text-shadow: none;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    transition: all 0.3s ease;
}

/* 当前高亮消息：居中放大，保持高度不变 */
.message-item.current {
    opacity: 1;
    font-size: 24px;
    font-weight: bold;
    text-shadow: 0 0 10px currentColor;
}

/* 消息颜色 */
.message-item.red-msg {
    color: #ff4d4d;
}
.message-item.yellow-msg {
    color: #ffd93e;
}
.message-item.green-msg {
    color: #4caf50;
}
</style>