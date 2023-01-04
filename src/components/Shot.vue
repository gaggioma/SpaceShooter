<template>
    
    <div class="shotClass" :style="{left: props.x + 'px', top: yState + 'px', height: props.height + 'px', width: props.width + 'px'}">
        <img :src="rocket" :style="{width: '100%'}"/>
    </div>
    
  
</template>

<script setup>

    import { ref } from 'vue'

    import rocket from "@/assets/rocket.png"

    const props = defineProps([
        "id", 
        "x", 
        "y",
        "height",
        "width"
    ])

    //Shot event
    const emit = defineEmits(['shotMove'])

    //Y step
    const yStep = ref(5);

    //y state
    const yState = ref(props.y)

    //Increment y every 5ms
    window.setInterval(() => {
        var newYvalue = yState.value - yStep.value 
        yState.value = newYvalue;
        emit("shotMove", {id: props.id, x: props.x, y: newYvalue})
    }, 20)

</script>

<style scoped>

    .shotClass{
        position: absolute;
        display: flex;
        justify-content: center;
        
    }

</style>