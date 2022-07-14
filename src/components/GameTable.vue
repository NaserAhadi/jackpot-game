<template>
    <div>
        <main class="container">
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
            <section class="button-wrapper">
                <button  
                    @click="startGame()"
                    class="start-button button"
                >
                    Start
                </button>
                <button 
                    @click="cashOut()"
                    class="cash-out-button button"
                >
                    Cash Out
                </button>
            </section>
          
        </main>
        <section class="score-wrapper">
            <div class="score">
                <span>
                    credit:  {{computeUserCredit}}
                </span>
                <span>
                    user cash: {{userCash? `${userCash} $`: '-'}}
                </span>
            </div>
        </section>
    </div>
</template>

<script>
import CherrySvg from '../assets/svg/cherry.svg'
import LemonSvg from '../assets/svg/lemon.svg'
import OrangeSvg from '../assets/svg/orange.svg'
import WaterMelonSvg from '../assets/svg/watermelon.svg'

export default {
    name: 'GameTable',
    computed:{
        computeUserCredit(){
            return this.credit
        }
    },
    components:{
        CherrySvg,
        LemonSvg,
        OrangeSvg,
        WaterMelonSvg
    },
    data(){
        return{
            list: ['CherrySvg','LemonSvg', 'OrangeSvg', 'WaterMelonSvg'],
            firstBlockIndex: null,
            secondBlockIndex: null,
            thirdBlockIndex: null,
            credit: 10,
            userCash: null
        }
    },
    methods:{
            generateRandomNumber(min, max){
                return Math.floor(Math.random() * (max - min + 1) + min)
            },
            startGame(){
                this.allocateRandomNumbers()
                this.checkUserIsWinner()
            },
            allocateRandomNumbers(){
                this.firstBlockIndex = this.generateRandomNumber(0,3)
                this.secondBlockIndex = this.generateRandomNumber(0,3)
                this.thirdBlockIndex = this.generateRandomNumber(0,3)
            },
            checkUserIsWinner(){
                if(this.firstBlockIndex===this.secondBlockIndex && this.firstBlockIndex===this.thirdBlockIndex && this.firstBlockIndex!==null){
                    this.giveReward()
                } else {
                    this.credit --
                }
            },
            giveReward(){
                if(this.list[this.firstBlockIndex] === 'CherrySvg'){
                    this.credit += 10
                } else if(this.list[this.firstBlockIndex] === 'LemonSvg') {
                    this.credit += 20
                } else if(this.list[this.firstBlockIndex] === 'OrangeSvg') {
                    this.credit += 30
                } else {
                    this.credit += 40
                }
            },
            cashOut(){
                this.userCash = this.credit
                this.credit = 0
            }
    }
}
</script>

<style scoped>
.container{
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
    width: 30%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.button-wrapper{
    display: flex;
    flex-direction: column;
    height: 100%;
    justify-content: space-between;
}

.button{
    color: #FFF;
    border: none;
    border-radius: 0.5rem;
    padding: 0.5rem 1rem;
    margin-left: 0.5rem;
    cursor: pointer;
    width: 6rem;
}

.start-button{
    background: #43CBC0;
}

.cash-out-button{
    background: #3F5469;
}

.start-button:hover{
    filter: brightness(95%);
}

.score-wrapper{
    width: 80vw;
    margin: auto;
}

.score{
    display:flex;
    justify-content: space-between;
    background: #F6F8FA;
    width: 88.5%;
    padding: 1rem;
}
</style>