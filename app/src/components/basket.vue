<template>
    <div class='basket'>
        <div class='basket__diagram'>
            <Charts class='basket__diagram_pie' :chart-data="getOptions"/>
            <p class='basket__diagram_not' v-if='this.items.length==0'>Заполните поля для Online расчета</p>
            <p class='basket__diagram_summ' v-else>{{Intl.NumberFormat('ru-RU').format(animatedNumber)}} ₽</p>
        </div>

        <div v-if='this.items.length!=0' class='fullInfo'>
            <div class='fullInfo__items'>
                <h4 class='fullInfo__header'>Стоимость полиса</h4>
                <transition-group name="list" tag="ul">
                    <div class='fullInfo__item' v-for='(item,index) in getMain' :key='index'>
                        <p class='fullInfo__label'>{{item.label}}</p>
                        <div class='fullInfo__separator'/>
                        <div :style='{backgroundColor:colorsArray[items.indexOf(item)]}' class='fullInfo__price'>{{item.price}} ₽</div>
                    </div>
                </transition-group>
            </div>

            <div class='fullInfo__items'>
                <h4 class='fullInfo__header'>Дополнительные услуги</h4>
                 <transition-group name="list" tag="ul">
                    <div class='fullInfo__item' v-for='(item,index) in getAddition' :key='index'>
                        <p class='fullInfo__label'>{{item.label}}</p>
                        <div class='fullInfo__separator'/>
                        <div :style='{backgroundColor:colorsArray[items.indexOf(item)]}' class='fullInfo__price'>{{item.price}} ₽</div>
                    </div>
                </transition-group>
            </div>
        </div>
    </div>
</template>



<script>
import Charts from './pieChart.vue'
import {gsap} from "gsap";



export default {
    components:{
        Charts 
    },
    computed:{
        getOptions(){
            return {
                datasets: [{
                label: '# of Votes',
                data: this.getPrice,
                backgroundColor: this.colorsArray,
                borderWidth: 0,
                }],        
            }
        },

        getPrice(){
            if(this.items.length!=0) return this.items.map((item)=>{return item.price})
            else return [1]
        },

        getMain(){
            return this.items.filter((item)=>{return item.type=='main'})
        },

        getAddition(){
            return this.items.filter((item)=>{return item.type=='addition'})
        },

        animatedNumber: function() {
            return this.tweenedNumber.toFixed(2);
        }
    },

    data(){
        return{
            tweenedNumber: 0,
            colorsArray:['#ffba51','#175351','#749897','#c1cecb','#FFD99F','#A66F1A','#BF995D']
        }
    },

    watch: {
        summ: function(newValue) {
        gsap.to(this.$data, { duration: 0.5, tweenedNumber: newValue });
        }
    },

    props:{
        summ:{type:Number,default:0},
        items:{type:Array,default:new Array()} 
    },

    methods:{
        shuffle(array) {
            var currentIndex = array.length, temporaryValue, randomIndex;

            while (0 !== currentIndex) {

                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex -= 1;

                temporaryValue = array[currentIndex];
                array[currentIndex] = array[randomIndex];
                array[randomIndex] = temporaryValue;
            }

            return array;
        }
    }
}
</script>

<style lang="scss">
    .list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active до версии 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>

<style lang="scss" scoped>
    .basket{
        display:flex;
        flex-direction: column;
        align-items:center;
        width:100%;
        &__diagram{
            display:flex;
            align-items:center;
            justify-content: center;
            position:relative;
            width:200px;
            &_pie{
                 width:200px;
            }

            &_not{
                text-align: center;
                position:absolute;
                width:80%;
                font-size: 14px;
                line-height: 20px;
                color: #4f4f4f;
                font-weight: 400;
                margin:0px;
            }

            &_summ{
                text-align: center; 
                position:absolute;
                font-size: 24px;
                line-height: 20px;
                color: #4f4f4f;
                font-weight: 400;
                width:80%;
                margin:0px;
            }
        }
    }
    .fullInfo{
        display:flex;
        flex-direction: column;
        width:100%;

        &__items{
            display:flex;
            flex-direction: column;
            align-items: center;
            ul{
                width:100%;
            }
        }

        &__item{
            display:flex;
            align-items: center;
            justify-content: space-between;
            width:100%;
            margin-bottom:10px;
        }

        &__header{
            font-weight: 400;
            font-size: 17px;
            line-height: 20px;
        }

        &__price{
            border-radius: 10px;
            text-align: center;
            min-width: 55px;
            font-weight: 400;
            color: #fff;
            font-size: 11px;
            line-height: 14px;
            padding: 3px 10px;
        }

        &__label{
            line-height: 15px;
            margin: 0;
            font-size: 11px;
            color: #4f4f4f;
            font-weight: 400;
        }
        &__separator{
            display:flex;
            flex: 1 0;
            margin:4px;
            margin-top:10px;
            border-bottom: 1px dotted #4f4f4f54;
        }

    }
</style>