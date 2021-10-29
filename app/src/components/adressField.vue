<template>
    <div class='adressField'>
        <div class='adressField__inputs'>
            <FrameUI class='adressField__input1' :name='name+"_frame1"' :description='!freeForm ? description:null' :errorText='errorText' :hasError='hasError' :required='required'  v-bind="$props" :focused='focused'>
                <input  :id='name+"_input1"' @focus="focused = true" @blur="focused = false" v-model='input1' class="text-field__input" type="text">
            </FrameUI>

            <FrameUI v-if='freeForm' class='adressField__input2' label='№ квартиры'   :name='name+"_frame2"' :description='description' :focused='focused'  :required='required'>
                <input :id='name+"_input2"' @focus="focused = true" @blur="focused = false" v-model='input2' class="text-field__input" type="text">
            </FrameUI>
        </div>

        <CheckBox class='adressField__checkbox' v-model='freeForm' label='Ввести адресс в свободной форме'   :name='name+"_checkBox"'/>
    
        <!-- <transition name="fade"> -->
                <div class=list>
                    <ul class=list__body>
                        <li  v-on:click='selectInput(option)' class="list__item" v-for='option in autoCompleteArray' :key='option.value'>
                                    <p class='list__item_text'>{{option.label}}</p>
                                    <div class='badge' v-if='option.isOnline'>Онлайн</div>
                        </li>
                        <p class="list__item_not" v-if='autoCompleteArray.length==0'>Не найдено совпадений</p>
                    </ul>
                </div>
        <!-- </transition> -->
    </div>
</template>

<script>

import FrameUI from './frame.vue'
import CheckBox from './checkbox.vue'

export default {
    components:{
        FrameUI,
        CheckBox
    },

    data(){
        return{
            focused:false,
            isShowDescription:false,
            freeForm:false,
            input1:'',
            input2:'',
            autoCompleteArray:[]
        }
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
        value:{type:Object,default:null},
    },
    watch:{
        input1:function(val){
            this.inputVal.valueText=val
        },
        input2:function(val){
            this.inputVal.valueFlat=val
        },
        freeForm:function(val){
            this.inputVal.isText=val

            if(!val){
                delete this.inputVal.isText
                delete this.inputVal.valueAddress
            }

        }
    },
    methods:{
        
    }

}
</script>

<style lang="scss">

    .fade-enter-active, .fade-leave-active {
    transition: opacity .3s;
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active до версии 2.1.8 */ {
    opacity: 0;
    }
</style>


<style lang="scss" scoped>
@import '@/scss/main.scss';
    .adressField{
        width:100%;
        &__inputs{
            display:flex;
            width:100%;
        }
        &__input1{
            width:70%;
        }
        &__input2{
            width:30%;
            margin-left:20px;
        }
        &__checkbox{
            max-width:210px;
            margin-top:30px;
        }
    }

    .list{
        z-index:999;
        position:absolute;
        overflow: auto;
        scrollbar-color: #458245 #714826;     /* «цвет ползунка» «цвет полосы скроллбара» */
        scrollbar-width: 5px;
        display:flex;
        width:100%;
        border:none;
        top:40px;
        left:-2px;
        border:2px solid transparent;
        border-radius: 10px;
        box-shadow: 0px 5px 10px 0 rgba(0, 0, 0, 0.05);
        max-height:308px;
        background:white;
        & > div{
            padding:0px !important;
        }
        &__body{
            padding:0px;
            list-style: none;
            width:100%;
        }
        &__item{
            
            display:flex;
            align-items:center;
            justify-content: space-between;
            padding:10px 15px;
            cursor:pointer;
            font-size: 14px;

            &_text{
                padding:0px;
                margin:0px;
                color: $focusColor1;
                line-height: 20px;
                text-transform: uppercase;
            }
            &_not{
                margin-left:20px;
                font-size: 12px;
                color: $focusColor1; 
            }

            &:hover{
                background-color:$focusColor1;
                .list__item_text{
                    color:white;
                }
            }
        }
        &:hover{
            background:$laptopColor
        }
    }
</style>