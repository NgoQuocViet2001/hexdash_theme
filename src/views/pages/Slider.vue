<template>
    <div class="slide-wrapper">
        <sdButton type="primary" size="lg" @click="handlePrevClick" class="slide-button slide-button-prev">
            <unicon name="angle-left-b" class="slide-icon"></unicon>
        </sdButton>
        <div :class="`slide-group slide-group-${id}`">
            <div v-for="item in arrayData" class="slide-item">{{ item }}</div>
        </div>
        <sdButton type="primary" size="lg" @click="handleNextClick" class="slide-button slide-button-next">
            <unicon name="angle-right-b" class="slide-icon"></unicon>
        </sdButton>
    </div>
</template>
  
<script setup lang="ts">

const props = defineProps(['id', 'arrayData', 'class', 'handlePrevClick', 'handleNextClick']);

</script>
  
<style>
.slide-wrapper {
    display: flex;
}

.slide-button {
    border-radius: 50% !important;
    padding: 12px   !important;
    background-color: #ae65f7 !important;
}

.slide-button:hover {
    background-color: #8231d3 !important;
}

.slide-button-prev:hover {
    transform: translateX(-4px) scale(1.1);
}

.slide-button-next:hover {
    transform: translateX(4px) scale(1.1);
}

.slide-group {
    display: flex;
    overflow: hidden;
    width: 42vw;
    margin-left: 10px;
    margin-right: 10px;
}

.slide-item {
    flex: 0 0 auto;
    background-color: #8231d3;
    color: white;
    padding: 10px 20px;
    border-radius: 20px;
    margin-right: 10px;
    transition: transform 0.3s ease-in-out;
}
</style>