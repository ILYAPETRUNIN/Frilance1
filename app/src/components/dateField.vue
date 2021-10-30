<template>
        <FrameUI v-bind="$props" :focused='focused'> 
            <DatePicker color="red" mode="date"  :min-date='minDate' :max-date='maxDate' v-model="inputVal">
                <template class='asd' v-slot="{ inputValue, inputEvents }">
                    <div class='dataPicker__input' v-on="inputEvents">
                        <input
                        v-mask="'##.##.####'"
                        @focus="focused = true" 
                        @blur="focused=false"
                        :id='name'
                        class="text-field__input"
                        :value="inputValue"
                        />
                        <div  class='text-field__dropDown' @click="active=!active" >
                            <svg  xmlns="http://www.w3.org/2000/svg" fill='#749897' width='20px' height='20px' viewBox="0 0 18 20" id="date-range"><path fill-rule="evenodd" clip-rule="evenodd" d="M6 9H4v2h2V9zm4 0H8v2h2V9zm4 0h-2v2h2V9zm2-7h-1V0h-2v2H5V0H3v2H2C.89 2 .01 2.9.01 4L0 18a2 2 0 002 2h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm0 16H2V7h14v11z"></path></svg>
                        </div>
                    </div>
                </template>
            </DatePicker>
    </FrameUI>
</template>

<script>
import FrameUI from './frame.vue'
import {VueMaskDirective} from 'v-mask'
import DatePicker from 'v-calendar/lib/components/date-picker.umd'


export default {
    data(){
        return{
            focused:false,
            active:false,
        }
    },
    components:{
        FrameUI,
        DatePicker
    },

    directives: {
         mask: VueMaskDirective,
    },
    computed:{
        inputVal:{
            get(){
                return this.value
            },
            set(val){
                this.$emit('input',val)      
            }
        }
    },
    props:{
        name:{type:String},
        label:{type:String},
        errorText:{type:String},
        hasError:{type:Boolean},
        required:{type:Boolean},
        description:{type:String},
        value:{type:Date,default:new Date()},
        placeholder:{type:String,default:null},
        minDate:{type:String,default:'01.01.1900'},
        maxDate:{type:String,default:'01.01.2050'} 
    },
}
</script>

<style lang="scss">
    @import '@/scss/main.scss';

    .vc-highlight{
        background:$inputColor!important;
        border-radius: 4px!important;
    }
    .vc-focusable{
        border-radius: 4px!important;
    }
</style>

<style lang="scss" scoped>
@import '@/scss/main.scss';

        .text-field{
            display:flex;
            flex-direction: column;
            align-items: flex-start;
            position:relative;
            width:100%;
            z-index:auto;
            &__input{
                width:100%;
            }

            &__dropDown{
                right:10px;
                position:absolute;
                fill:$inputColor;
                cursor:pointer;
                z-index:3;
                transition:transform 0.3s;
            }

            &__inner{
                span{
                    display:flex;
                    align-items:center;
                    width:100%;
                }
            }
        }

        .dataPicker{
            z-index:999;
            position:absolute;
            overflow: auto;
            display:flex;
            border:none;
            top:40px;
            left:-2px;
            border:2px solid transparent;
            border-radius: 10px;
            box-shadow: 0px 5px 10px 0 rgba(0, 0, 0, 0.05);
            max-height:308px;
            background:white;
            &__input{
                display:flex;
                align-items:center;
                width:100%;
            }
        }
</style>

