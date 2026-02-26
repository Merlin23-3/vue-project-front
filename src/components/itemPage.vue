<template>
    <div class="item">
        <slot>

        </slot>     
    </div>
</template>

<script setup lang="js">
</script>
    
<style>
.item {
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
}

.item > * {
    flex: 1;
    width: 100%;
    min-height: 0;
}
</style>