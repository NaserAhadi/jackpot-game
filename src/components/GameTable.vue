<template>
    <div>
        <main class="container">
            <span class="fruit-block" :class="{'winner-style': isWinner}">
                <Loading v-if="firstBlockLoading" />
                <component 
                    v-else-if="firstBlockIndex!==null" 
                    :is="list[firstBlockIndex]" 
                    class="fruit-component"
                />
                <span v-else>start the game</span>
            </span>
            <span class="fruit-block" :class="{'winner-style': isWinner}"> 
                <Loading v-if="secondBlockLoading" />
                <component 
                    v-else-if="secondBlockIndex!==null" 
                    :is="list[secondBlockIndex]" 
                    class="fruit-component"
                />
                <span v-else>start the game</span>
            </span>
            <span class="fruit-block" :class="{'winner-style': isWinner}">
                <Loading v-if="thirdBlockLoading" />
                <component 
                    v-else-if="thirdBlockIndex!==null" 
                    :is="list[thirdBlockIndex]" 
                    class="fruit-component"
                />
                <span v-else>start the game</span>
            </span>
            <button  
                @click="startGame()"
                class="start-button button"
            >
                Start
            </button>
        </main>
        <section class="score-wrapper">
            <div class="score" :class="{'winner-score-style': isWinner}">
                <span>
                    Credit:  {{computeUserCredit}}
                </span>
                <span>
                    User Cash: {{userCash? `${userCash} $`: '-'}}
                </span>
            </div>
        </section>
        <section class="cashout-button-wrapper">
            <button 
                @click="cashOut()"
                @mouseover="setRandomPosiblitiesOnCashOutbutton()"
                class="cash-out-button button"
                :class="[translateClassName, {'disable-button':isDisableCashoutButton}]"
            >
                Cash Out
            </button>
        </section>
    </div>
</template>

<script>
import CherrySvg from '../assets/svg/cherry.svg'
import LemonSvg from '../assets/svg/lemon.svg'
import OrangeSvg from '../assets/svg/orange.svg'
import WaterMelonSvg from '../assets/svg/watermelon.svg'
import Loading from './Loading.vue'

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
        WaterMelonSvg,
        Loading
    },
    data(){
        return{
            list: ['CherrySvg','LemonSvg', 'OrangeSvg', 'WaterMelonSvg'],
            firstBlockIndex: null,
            secondBlockIndex: null,
            thirdBlockIndex: null,
            credit: 10,
            userCash: null,
            isTranslateButton: false,
            translateClassName: '',
            translateClassNamesList: ['translate-plus-x', 'translate-minus-x', 'translate-plus-y', 'translate-minus-y'],
            isDisableCashoutButton: false,
            isWinner: false,
            firstBlockLoading: false,
            secondBlockLoading: false,
            thirdBlockLoading: false,
        }
    },
    watch:{
        credit(newCreditValue){
            if(newCreditValue===0 && !this.userCash){
                this.setNotification({
                        title: 'There is no credit for playing the game, you lose the game',
                        type: 'error'
                    })
            }
        },
    },
    methods:{
            generateRandomNumber(min, max){
                return Math.floor(Math.random() * (max - min + 1) + min)
            },
            startGame(){
                if(this.credit>0){
                    this.allocateRandomNumbers()
                    this.activeLoadings()
                    this.setOffWinningState()
                } else {
                    this.setNotification({
                        title: 'There is no credit for playing the game',
                        type: 'warn'
                    })
                }
            },
            async allocateRandomNumbers(){
                let firstIndex = await this.generateRandomNumber(0,3)
                let secondIndex = await this.generateRandomNumber(0,3)
                let thirdIndex = await this.generateRandomNumber(0,3)
                if(firstIndex===secondIndex && firstIndex===thirdIndex){
                    console.log(firstIndex, secondIndex, thirdIndex);
                    let raondomNumber = Math.random()
                    if(this.credit> 40 && this.credit<60){
                        if(raondomNumber<0.3){
                            this.allocateRandomNumbers()
                            console.log('start game', raondomNumber);
                        } else {
                            this.setBlocksIndexs(firstIndex, secondIndex, thirdIndex)
                        }
                   } else if (this.credit > 60){
                      if(raondomNumber<0.6){
                            this.allocateRandomNumbers()
                            console.log('start game', raondomNumber);
                        } else {
                            this.setBlocksIndexs(firstIndex, secondIndex, thirdIndex)
                        }
                   } else {
                        this.setBlocksIndexs(firstIndex, secondIndex, thirdIndex)
                   }
                } else {
                    this.setBlocksIndexs(firstIndex, secondIndex, thirdIndex)
                }
            },
            setBlocksIndexs(firstIndex, secondIndex, thirdIndex){
                setTimeout(() => {
                    this.firstBlockIndex = firstIndex
                    this.deactiveFirstBlockLoading()
                }, 1000)
                setTimeout(() => {
                    this.secondBlockIndex = secondIndex
                    this.deactiveSecondBlockLoading()
                }, 2000)
                setTimeout(() => {
                    this.thirdBlockIndex = thirdIndex
                    this.deactiveThirdBlockLoading()
                    this.checkUserIsWinner()
                }, 3000)
            },
            activeLoadings(){
                this.firstBlockLoading = true 
                this.secondBlockLoading = true
                this.thirdBlockLoading = true
            },
            deactiveFirstBlockLoading(){
                this.firstBlockLoading = false
            },
            deactiveSecondBlockLoading(){
                this.secondBlockLoading = false
            },
            deactiveThirdBlockLoading(){
                this.thirdBlockLoading = false
            },
            checkUserIsWinner(){
                if(this.firstBlockIndex===this.secondBlockIndex && this.firstBlockIndex===this.thirdBlockIndex && this.firstBlockIndex!==null){
                    this.giveReward()
                        this.setWinningState()
                        this.setNotification({
                            title: 'You Win, Congratulation',
                            type: 'success'
                        })
                } else {
                    this.reduceCredit()
                    this.setOffWinningState()
                }
            },
            reduceCredit(){
                this.credit --
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
            setWinningState(){
                this.isWinner = true
            },
            setOffWinningState(){
                if(this.isWinner){
                    this.isWinner = false
                }
            },
            cashOut(){
                if(!this.isDisableCashoutButton){
                    if(this.credit>0){
                        this.userCash = this.credit
                        this.credit = 0
                        this.disableCashoutButton()
                    } else {
                        this.setNotification({
                            title: 'There is no credit for cashing out',
                            type: 'warn'
                        })
                        this.disableCashoutButton()
                    }
                }
            },
            setRandomPosiblitiesOnCashOutbutton(){
                let raondomNumber = Math.random()
                if(raondomNumber<0.5){
                    this.enableCashoutButton()
                    this.translateClassName = this.translateClassNamesList[this.generateRandomNumber(0,3)]
                    this.isTranslateButton = true
                } else if(raondomNumber<0.9){
                    this.disableCashoutButton()
                } else {
                    this.enableCashoutButton()
                }
            },
            disableCashoutButton(){
                this.isDisableCashoutButton = true
            },
            enableCashoutButton(){
                this.isDisableCashoutButton = false
            },
            setNotification({title, type,}){
                    this.$notify({
                        title,
                        type
                    });
            }
    }
}
</script>

<style scoped>
.container{
    height: 60vh;
    width: 80vw;
    margin: 8rem auto auto;
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

.winner-style{
    border: thin solid #43CBC0;
}

.cashout-button-wrapper{
    display: flex;
    justify-content: center;
    margin: 2rem auto;
    padding: 0.5rem;
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

.disable-button{
    background: #D4d4d4 !important;
    cursor: default;
}

.start-button{
    background: #43CBC0;
}

.cash-out-button{
    background: #3F5469;
}

.translate-plus-x{
    transform: translate(300px)
}

.translate-minus-x{
    transform: translate(-300px)
}

.translate-plus-y{
    transform: translate(0,300px)
}

.translate-minus-y{
    transform: translate(0,-300px)
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

.winner-score-style{
    color: #43CBC0;
}
</style>