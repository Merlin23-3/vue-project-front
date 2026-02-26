<template>
    <div>
        <div dv-bg class="item-five-container">
            <!-- 标题和刷新按钮 -->
            <div class="header">
                <button class="refresh-btn" @click="refreshAllData">
                    <span class="refresh-icon">↻</span>
                </button>
                <h2>实时内容展示</h2>
            </div>

            <!-- 六个数据卡片网格 -->
            <div class="cards-grid">
                <div
                    v-for="item in gaugeList"
                    :key="item.id"
                    class="data-card"
                    :style="{ backgroundColor: item.color + '20' }"
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
import { ref, onMounted} from 'vue';

// 六个数据项配置
const gaugeList = ref([
    { id: 'temp', name: '温度', unit: '℃', value: 0, min: 15, max: 25, color: '#ff6b6b' },
    { id: 'salinity', name: '盐度', unit: 'PSU', value: 0, min: 28, max: 35, color: '#4dabf7' },
    { id: 'flow', name: '流速', unit: 'm/s', value: 0, min: 0, max: 2.0, color: '#51cf66' },
    { id: 'wind', name: '风速', unit: 'm/s', value: 0, min: 0, max: 15, color: '#ffd43b' },
    { id: 'wave', name: '波高', unit: 'm', value: 0, min: 0, max: 5, color: '#339af0' },
    { id: 'eddy', name: '涡旋', unit: '个', value: 0, min: 0, max: 30, color: '#ae3ec9' }
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

// 初始数据
onMounted(() => {
    refreshAllData();
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
    height: 40px; /* 固定高度，与之前比例一致 */
    padding: 0 10px;
    flex-shrink: 0;
}

.refresh-btn {
    background: transparent;
    border: none;
    color: #00f2fe;
    font-size: 24px;
    cursor: pointer;
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: all 0.2s;
    margin-right: 5px;
}

.refresh-btn:hover {
    background: rgba(0, 242, 254, 0.2);
    transform: rotate(30deg);
}

.header h2 {
    color: white;
    margin: 0;
}

.refresh-icon {
    display: inline-block;
    transition: transform 0.3s;
}

.refresh-btn:hover .refresh-icon {
    transform: rotate(180deg);
}

h2 {
    color: #cd1a;
    line-height: 36px;
    font-size: 14px;
    margin: 0;
    text-align: center;
    flex: 1;
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