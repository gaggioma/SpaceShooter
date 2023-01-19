<template>

<div class="spacecraftClass" :style="{left: left + 'px', top: top + 'px', width: spacecraftWidth + 'px', height: spacecraftHeight + 'px'}">
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
    "arenaHeight",
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

//varing margin to, spacecraft will move up or down
const top = ref(props.arenaHeight - props.height)

//Timer direction
const timerDirectionFunctionLeft = ref();
const timerDirectionFunctionRight = ref();
const timerDirectionFunctionUp = ref();
const timerDirectionFunctionDown = ref();

//Every time direction prop chenge move spacecraft left or right
watch(() => props.direction, (newVal) => {
    
    //Clear previous timer instance
    if(newVal.direction === "left" && newVal.state === "stop"){
        clearInterval(timerDirectionFunctionLeft.value);
        return null;
    }

    if(newVal.direction === "left" && newVal.state === "go"){
        clearInterval(timerDirectionFunctionLeft.value);
        timerDirectionFunctionLeft.value = window.setInterval(() => {
            if(left.value - movStep.value <= 0){
                left.value = 0
            }else{
                left.value = left.value - movStep.value;    
            }
        }, 10);
        return null;     
    }

    if(newVal.direction === "right" && newVal.state === "stop"){
        clearInterval(timerDirectionFunctionRight.value);
        return null;
    }

    if(newVal.direction === "right" && newVal.state === "go"){
        clearInterval(timerDirectionFunctionRight.value);
        timerDirectionFunctionRight.value = window.setInterval(() => {
            if(left.value + movStep.value + spacecraftWidth.value > props.arenaWidth){
                left.value = props.arenaWidth - spacecraftWidth.value;
            }else{
                left.value = left.value + movStep.value;
            }
        }, 10);
        return null;  
    }

    if(newVal.direction === "up" && newVal.state === "stop"){
        clearInterval(timerDirectionFunctionUp.value);
        return null;
    }

    if(newVal.direction === "up" && newVal.state === "go"){
        clearInterval(timerDirectionFunctionUp.value);
        timerDirectionFunctionUp.value = window.setInterval(() => {
            //upper bound condition
            if(top.value - movStep.value < 0){
                top.value = 0;
            }else{
                top.value = top.value - movStep.value;
            }
        }, 10);
        return null;         
    }

    if(newVal.direction === "down" && newVal.state === "stop"){
        clearInterval(timerDirectionFunctionDown.value);
        return null;
    }

    if(newVal.direction === "down" && newVal.state === "go"){
        clearInterval(timerDirectionFunctionDown.value);
        timerDirectionFunctionDown.value = window.setInterval(() => {
            //lower bound condition
            if(top.value + movStep.value + spacecraftHeight.value > props.arenaHeight){
                top.value = props.arenaHeight - spacecraftHeight.value;
            }else{
                top.value = top.value + movStep.value;
            }
        }, 10);
        return null;        
    }
});

//Shoot watcher
watch(() => props.shoot, (newValue) => {

    if(newValue === "fire"){
        //Create new shot and add it into shot array.
        //Every shot is identify by (rand, margin left)
        emit("shootEvent", 
        {
            id: Math.random(), 
            x: left.value  + (spacecraftWidth.value - 1)/2, 
            y: top.value
        });
    }
})

</script>

<style scoped>
    .spacecraftClass{
        position: absolute;
        /*bottom: 0px;*/
        display: flex;
        justify-content: center;
    }

</style>