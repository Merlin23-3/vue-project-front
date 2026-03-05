<template>
    <div>
        <div dv-bg class="item-five-container">
            <!-- 标题（无刷新按钮） -->
            <div class="header">
                <h2 class="artistic-title">实时内容展示</h2>  <!-- 使用艺术字体 -->
            </div>

            <!-- 六个数据卡片网格 -->
            <div class="cards-grid">
                <div
                    v-for="item in gaugeList"
                    :key="item.id"
                    class="data-card"
                    :style="{ backgroundColor: item.bgColor + '20' }"
                >
                    <div class="card-name">{{ item.name }}</div>
                    <div class="card-value" :style="{ color: item.color }">
                        {{ item.value }}<span class="unit">{{ item.unit }}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

// 六个数据项配置
const gaugeList = ref([
    { 
        id: 'temp', 
        name: '温度', 
        unit: '℃', 
        value: 0, 
        min: 15, 
        max: 25, 
        color: '#ff6b6b',        // 字体颜色
        bgColor: '#ff6b6b'        // 背景颜色（加20透明度）
    },
    { 
        id: 'salinity', 
        name: '盐度', 
        unit: 'PSU', 
        value: 0, 
        min: 28, 
        max: 35, 
        color: '#4dabf7',
        bgColor: '#4dabf7'
    },
    { 
        id: 'flow', 
        name: '流速', 
        unit: 'm/s', 
        value: 0, 
        min: 0, 
        max: 2.0, 
        color: '#51cf66',
        bgColor: '#51cf66'
    },
    { 
        id: 'wind', 
        name: '风速', 
        unit: 'm/s', 
        value: 0, 
        min: 0, 
        max: 15, 
        color: '#ae3ec9',        // 字体颜色改为紫色
        bgColor: '#ffd43b'        // 背景保持黄色
    },
    { 
        id: 'wave', 
        name: '波高', 
        unit: 'm', 
        value: 0, 
        min: 0, 
        max: 5, 
        color: '#339af0',
        bgColor: '#339af0'
    },
    { 
        id: 'eddy', 
        name: '涡旋', 
        unit: '个', 
        value: 0, 
        min: 0, 
        max: 30, 
        color: '#ffd43b',        // 字体颜色改为黄色
        bgColor: '#ae3ec9'        // 背景保持紫色
    }
]);

// 生成随机值
const generateRandomValue = (item) => {
    return +(item.min + Math.random() * (item.max - item.min)).toFixed(1);
};

// 刷新所有数据
const refreshAllData = () => {
    gaugeList.value.forEach(item => {
        item.value = generateRandomValue(item);
    });
};

let timer = null;

// 初始数据并启动定时器
onMounted(() => {
    refreshAllData();
    // 每5秒自动刷新
    timer = setInterval(() => {
        refreshAllData();
    }, 5000);
});

// 组件卸载时清除定时器
onUnmounted(() => {
    if (timer) {
        clearInterval(timer);
        timer = null;
    }
});
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

/* 如果字体文件在public目录下，使用这个路径 */
/* @font-face {
    font-family: '演示秋鸿楷';
    src: url('/fonts/演示秋鸿楷.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
    font-display: swap;
} */

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
    justify-content: center; /* 标题居中 */
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

/* 卡片网格：上三下三 */
.cards-grid {
    flex: 1;
    min-height: 0;
    display: grid;
    grid-template-rows: 1fr 1fr;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
    padding: 8px;
    background: transparent;
}

.data-card {
    border-radius: 12px;
    padding: 12px 8px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(4px);
    border: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
    transition: transform 0.2s;
}

.data-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 16px rgba(0, 242, 254, 0.2);
}

.card-name {
    color: #fff;
    font-size: 14px;
    margin-bottom: 8px;
    opacity: 0.9;
}

.card-value {
    font-size: 28px;
    font-weight: bold;
    line-height: 1.2;
    text-shadow: 0 0 10px currentColor;
}

.unit {
    font-size: 14px;
    margin-left: 4px;
    font-weight: normal;
    opacity: 0.7;
    color: #fff;
}
</style>