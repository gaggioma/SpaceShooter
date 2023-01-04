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
const directionState = ref("stop")

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

//When key up stop direction and shot
const handleKeyUp = (event) => {
    
    //Direction
    if(event.keyCode === 37 || event.keyCode === 39){
        directionState.value = "stop";
    }

    //Fire
    if(event.keyCode === 32){
        shootState.value = "stop";
    }
}

const handleKeyDown = (event) => {
    //console.log("key down: " + event.keyCode)
    if(event.keyCode === 39){
        directionState.value = "right";
    }

    if(event.keyCode === 37){
        directionState.value = "left";
    }

    if(event.keyCode === 32){
        shootState.value = "fire";
    }
}

//Handle spacecraft shot
const shootHandler = (shots) => {
    var shotArray = shotContainer.value;
    shots["y"] = arenaHeight.value - spacecraftHeight.value - shotHeight.value;
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