<template>
    <div class="switch-buttons">
        <button 
            v-for="item in chartItems" 
            :key="item.id"
            :class="['switch-btn', { active: activeId === item.id }]"
            @click="$emit('switch', item.id)"
        >
            {{ item.name }}
        </button>
    </div>
</template>

<script setup>
defineProps({
    activeId: {
        type: String,
        default: 'temp'
    },
    chartItems: {
        type: Array,
        default: () => [
            { id: 'temp', name: '温度' },
            { id: 'salinity', name: '盐度' },
            { id: 'flow', name: '流速' },
            { id: 'wind', name: '风速' },
            { id: 'wave', name: '波高' },
            { id: 'eddy', name: '涡旋' }
        ]
    }
})

defineEmits(['switch'])
</script>

<style scoped>
.switch-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
    padding: 8px;
    justify-content: center;
    background: rgba(0, 0, 0, 0.2);
    border-radius: 8px;
}

.switch-btn {
    padding: 4px 8px;
    border: 1px solid #00f2fe;
    background: transparent;
    color: #fff;
    border-radius: 4px;
    cursor: pointer;
    font-size: 12px;
    transition: all 0.3s;
}

.switch-btn:hover {
    background: #00f2fe;
    color: #000;
}

.switch-btn.active {
    background: #00f2fe;
    color: #000;
    border-color: #fff;
}
</style>