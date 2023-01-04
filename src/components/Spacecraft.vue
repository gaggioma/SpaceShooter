<template>

<div class="spacecraftClass" :style="{left: left + 'px', width: spacecraftWidth + 'px', height: spacecraftHeight + 'px'}">
    <img :src="spacecraftImage" :style="{width: '100%'}"/>
</div>
  
</template>

<script setup>

import { ref, watch } from 'vue'

import spacecraftImage from "@/assets/spacecraft.png"

//Components props
const props = defineProps([
    "direction",
    "shoot",
    "arenaWidth",
    "width",
    "height"
]);

//Background image
//const backgroundImageUrl = ref(require "url('@/assets/spacecraft.png')");

//Shot event
const emit = defineEmits(['shootEvent'])

//Spacecraft width
const spacecraftWidth = ref(props.width);

//Spacecraft width
const spacecraftHeight = ref(props.height);

//Moving step
const movStep = ref(5);

//Varing margin left, spacecraft will be moved left or right
const left = ref(props.arenaWidth / 2);

//Timer direction
const timerDirectionFunction = ref();

//Every time direction prop chenge move spacecraft left or right
watch(() => props.direction, (newVal) => {
    
    //Clear previous timer instance
    clearInterval(timerDirectionFunction.value);

    if(newVal === "left"){
        timerDirectionFunction.value = window.setInterval(() => {
            if(left.value - movStep.value <= 0){
                left.value = 0
            }else{
                left.value = left.value - movStep.value;    
            }
        }, 10);        
    }

    if(newVal === "right"){
        timerDirectionFunction.value = window.setInterval(() => {
            if(left.value + movStep.value + spacecraftWidth.value > props.arenaWidth){
                left.value = props.arenaWidth - spacecraftWidth.value;
            }else{
                left.value = left.value + movStep.value;
            }
        }, 10);         
    }
});

//Shoot watcher
watch(() => props.shoot, (newValue) => {

    if(newValue === "fire"){
        //Create new shot and add it into shot array.
        //Every shot is identify by (rand, margin left)
        emit("shootEvent", {id: Math.random(), x: left.value  + (spacecraftWidth.value - 1)/2});
    }
})

</script>

<style scoped>
    .spacecraftClass{
        position: absolute;
        bottom: 0px;
        display: flex;
        justify-content: center;
    }

</style>