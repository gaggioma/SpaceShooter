<template>

<div class="arenaWrapper">

    <div class="statisticsClass">
    {{ "Total enemies: " + totalEnemis }}
    {{ "Destroied enemies: " + destroiedCount }}
    {{ "Accuracy: " + Math.round(destroiedCount/totalEnemis * 100) + "%" }}
    </div>

    <div class="arenaClass" :style="{width: arenaWidth + 'px', height: arenaHeight + 'px'}">

        <Shot v-for="item in shotContainer" 
        :key="item.id"
        :id="item.id"
        :x="item.x"
        :y="item.y"
        :height="shotHeight"
        :width="shotWidth"
        @shotMove="shotMoveHandler"
        ></Shot>

        <Spacecraft
        :direction="directionState"
        :shoot="shootState"
        :arenaWidth="arenaWidth"
        :arenaHeight="arenaHeight"
        :width="spacecraftWidth"
        :height="spacecraftHeight"
        @shootEvent="shootHandler">
        </Spacecraft>

        <Enemy v-for="enemy in enemyList"
        :key="enemy.id"
        :x="enemy.x"
        :y="enemy.y"
        :width="enemy.width"
        :height="enemy.height"    
        ></Enemy>

    </div>

</div>
 
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

//Spacecraft
import Spacecraft from "@/components/Spacecraft.vue"

//Shot
import Shot from "@/components/Shot.vue"

//Enemy
import Enemy from "@/components/Enemy.vue"

//Arena width
const arenaWidth = ref(1000)

//Arena height
const arenaHeight = ref(800)

//Direction state
const directionState = ref({direction: "all", state: "stop"})

//Shot state
const shootState = ref("stop")

//Shots container
const shotContainer = ref([])

//Shot dimensione
const shotHeight = ref(55)
const shotWidth = ref(31)

//Spacecraft dimensions in pixels
const spacecraftWidth = ref(61);
const spacecraftHeight = ref(70);

//Enemy list
const enemyList = ref([])

//Destroied enemy count
const destroiedCount = ref(0)
const totalEnemis = ref(0)

//When key up stop ONLY shot. Directions continue to drive spacecraft until bound reach.
const handleKeyUp = (event) => {
    
    //Fire stop
    if(event.keyCode === 32){
        shootState.value = "stop";
    }

    if(event.keyCode === 37){
        directionState.value = {direction: "left", state: "stop"};
    }

    if(event.keyCode === 38){
        directionState.value = {direction: "up", state: "stop"};
    }

    if(event.keyCode === 39){
        directionState.value = {direction: "right", state: "stop"};
    }

    if(event.keyCode === 40){
        directionState.value = {direction: "down", state: "stop"};
    }
}

const handleKeyDown = (event) => {

    if(event.keyCode === 32){
        shootState.value = "fire";
    }

    if(event.keyCode === 37){
        directionState.value = {direction: "left", state: "go"};
    }

    if(event.keyCode === 38){
        directionState.value = {direction: "up", state: "go"};
    }

    if(event.keyCode === 39){
        directionState.value = {direction: "right", state: "go"};
    }

    if(event.keyCode === 40){
        directionState.value = {direction: "down", state: "go"};
    }
}

//Handle spacecraft shot
const shootHandler = (shots) => {
    var shotArray = shotContainer.value;
    shots["y"] = shots.y;
    shots["x"] = shots.x - (shotWidth.value - 1)/2;
    shotArray.push(shots)
    shotContainer.value = shotArray;
}

//Handle shot movement
const shotMoveHandler = (shot) => {

    var shotArray = shotContainer.value;
    
    //Remove shot when out of arena bounds
    if(shot.y < 0){
        shotArray = shotArray.filter((elem)=>{
            return elem.id !== shot.id
        });
        console.log("Shot: " + shot.id + " removed!")
        shotContainer.value = shotArray;
        return null;
    }

    //Find impacted enemy
    var enemyArray = enemyList.value
    var impactedEnemy = enemyArray.find((enemy) => {
        return (
            (shot.y < enemy.y + enemy.height)
            &&
            !(
                (shot.x > enemy.x + enemy.width) || 
                (shot.x + shotWidth.value < enemy.x)
            )
        )
    });

    //If enemy has been impacted remove enemy and shot
    if(impactedEnemy != undefined){
        //Remove enemy
        enemyArray = enemyArray.filter((elem) => {
            return elem.id != impactedEnemy.id
        });
        enemyList.value = enemyArray;

        //Remove shot
        shotArray = shotArray.filter((elem)=>{
            return elem.id !== shot.id
        });
        shotContainer.value = shotArray;
        
        //Update destroied enemies count
        destroiedCount.value = destroiedCount.value + 1;
        return null;
    }

}

onMounted(() => {
    window.addEventListener('keyup', handleKeyUp, null);
    window.addEventListener('keydown', handleKeyDown, null);

    //Every 5 sec add an enemy uniformly on arena width
    window.setInterval(() => {
        var enemyArray = enemyList.value;
        enemyArray.push({id: Math.random(), x: Math.floor(Math.random() * (arenaWidth.value - 50)), y: 0, height: 50, width: 50})
        totalEnemis.value = totalEnemis.value + 1;
    }, 200);
    

    //enemis scroll down
    window.setInterval(() => {
        
        var outOfBoundEnemy = [];

        var enemyArray = enemyList.value;
        for(let i=0; i<enemyArray.length; i++){

            //Increment y value
            enemyArray[i].y = enemyArray[i].y + 10;
            
            //If out of bounds register id
            if(enemyArray[i].y > arenaHeight.value){
                outOfBoundEnemy.push(enemyArray[i].id);
            }
        }

        //Remove out of bounds
        outOfBoundEnemy.forEach((enemyId) => {
                enemyArray = enemyArray.filter((enemy) => {
                    return enemy.id != enemyId
                })
        })
        enemyList.value = enemyArray;
    
    }, 50);

});

onUnmounted(() => {
    window.removeEventListener('keyup', handleKeyUp);
    window.removeEventListener('keydown', handleKeyDown);
});

</script>

<style scoped>
    .arenaClass{
        position: relative;
        border-color: aqua;
        border-style: solid;
        margin-bottom: 110px;
    }

    .arenaWrapper{
        display: flex;
        flex-direction: column;
        background-color: black;
    }

    .statisticsClass{
        text-align: center;
        color: white;
    }


</style>