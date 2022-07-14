<template>
    <div class="container">
        <span class="fruit-block">
            <component 
                v-if="firstBlockIndex!==null" 
                :is="list[firstBlockIndex]" 
                class="fruit-component"
            />
            <span v-else>-</span>
        </span>
        <span class="fruit-block"> 
             <component 
                v-if="secondBlockIndex!==null" 
                :is="list[secondBlockIndex]" 
                class="fruit-component"
            />
            <span v-else>-</span>
        </span>
        <span class="fruit-block">
             <component 
                v-if="thirdBlockIndex!==null" 
                :is="list[thirdBlockIndex]" 
                class="fruit-component"
            />
            <span v-else>-</span>
        </span>
        <button @click="startGame()">start game</button>
    </div>
</template>

<script>
import LemonSvg from '../assets/svg/lemon.svg'
import CherrySvg from '../assets/svg/cherry.svg'
import OrangeSvg from '../assets/svg/orange.svg'
import WaterMelonSvg from '../assets/svg/watermelon.svg'

export default {
    name: 'GameTable',
    computed:{
    },
    components:{
        LemonSvg,
        CherrySvg,
        OrangeSvg,
        WaterMelonSvg
    },
    data(){
        return{
            list: ['LemonSvg', 'CherrySvg', 'CherrySvg', 'WaterMelonSvg'],
            firstBlockIndex: null,
            secondBlockIndex: null,
            thirdBlockIndex: null,
        }
    },
    methods:{
            generateRandomNumber(min, max){
            return Math.floor(Math.random() * (max - min + 1) + min)
        },
        startGame(){
            this.firstBlockIndex = this.generateRandomNumber(0,3)
            this.secondBlockIndex = this.generateRandomNumber(0,3)
            this.thirdBlockIndex = this.generateRandomNumber(0,3)
        }
    }
}
</script>

<style scoped>
.container{
    border: 1px solid red;
    height: 60vh;
    width: 80vw;
    margin: auto;
    display: flex;
    align-items: center;
}

.fruit-component{
    width: 12rem;
    height: 12rem;
}

.fruit-block{
    border: thin solid gray;
    width: 32%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>