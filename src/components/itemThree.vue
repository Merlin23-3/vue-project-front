<template>
    <div class="item-wrapper">
        <div dv-bg class="item-content" style="width: 100%; height: 100%;">
            <div class="chart" id="threechart"></div>
        </div>
    </div>
</template>
<script setup lang="js">
// import { inject ,onMounted} from 'vue';
// let $echarts=inject("echarts")

</script>

<style>
.item-wrapper {
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
    padding-left: 0;
    margin: 0;
    flex-shrink: 0;
}
</style>