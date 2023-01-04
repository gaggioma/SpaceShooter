<template>
    <div class="enemyClass" :style="{left: xState + 'px', top: yState + 'px', height: enemyHeight + 'px', width: enemyWidth + 'px'}">
        <img :src="image" :style="{width: '100%'}"/>
    </div>
</template>

<script setup>

import { ref, onMounted, watch } from "vue"

import santaclaus from "@/assets/santaclaus.png"
import epiphany from "@/assets/epiphany.png"

//Props: 
/*
x init position
y init position
id
*/
const props = defineProps(["id", "x", "y", "height", "width"])

//Write all props into state. If in the future you want
const xState = ref (props.x);
const yState = ref (props.y);
const idState = ref (props.id);

const enemyHeight = ref(props.height);
const enemyWidth = ref(props.width);

const image = ref()

watch(() => props.x, (newValue) => {
    xState.value = newValue;
});

watch(() => props.y, (newValue) => {
    yState.value = newValue;
});

watch(() => props.id, (newValue) => {
    idState.value = newValue;
});

onMounted(() => {

    if(Math.random() >= 0.5){
        image.value = epiphany;
    }else{
        image.value = santaclaus
    }
})

</script>

<style scoped>

    .enemyClass{
        position: absolute;
        display: flex;
        justify-content: center;
    }

</style>