<template>
    <FrameUI v-bind="$props"> 
        <input :id='name' :readonly='(!autocomplete)' @click="active=!active"  v-model='selected.label' class="text-field__input" type="text">
         <div class='text-field__dropDown' @click="active=!active" :class='{rotate180:active}'>
        <svg   xmlns="http://www.w3.org/2000/svg" fill='#749897' width='20px' height='20px' viewBox='0 0 12 20'><path d="M10.59 6.295L6 10.875l-4.59-4.58L0 7.705l6 6 6-6-1.41-1.41z"></path></svg>
        </div>

        <div v-if='multiple' class='text-field__multiple'>
                        <div class='text-field__multiple_item'  v-for='item in multipleSelected' :key='item.value'>
                            {{item.label}}
                            <div @click='deleteSelect(item)' class='text-field__multiple_icon'>
                                <svg fill='white' xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Layer_1" x="0px" y="0px" width="14px" height="14px" viewBox="0 0 14 20" enable-background="new 0 0 14 20" xml:space="preserve">
                                    <path d="M14,4.41L12.59,3L7,8.59L1.41,3L0,4.41L5.59,10L0,15.59L1.41,17L7,11.41L12.59,17L14,15.59L8.41,10L14,4.41z"/>
                                </svg>
                            </div>
                        </div>

        </div>

        <transition name="fade">
                        <div v-on-clickaway='closeList' v-if='active' class=list>
                            <ul class=list__body>
                                <li :class='{activeItem:checkIsSelect(option.value)}' v-on:click='select(option)' class="list__item" v-for='option in autoCompleteArray' :key='option.value'>
                                    <p class='list__item_text'>{{option.label}}</p>
                                    <div class='badge' v-if='option.isOnline'>Онлайн</div>
                                </li>
                                <p class="list__item_not" v-if='autoCompleteArray.length==0'>Не найдено совпадений</p>
                            </ul>
                        </div>
        </transition>
    </FrameUI>
</template>

<script>


import { directive as onClickaway } from 'vue-clickaway';
import FrameUI from './frame.vue'


export default {
    directives: {
         onClickaway: onClickaway,
    },

    components:{
        FrameUI
    },
    
    data(){
        return{
            active:false,
            isShowDescription:false,
            selected:{},
            multipleSelected:[]
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
        },

        autoCompleteArray(){
            if(this.autocomplete){
                if(Object.keys(this.selected).length!=0){
                    return this.options.filter((el)=>{
                        return el.label.toLowerCase().includes(this.selected.label.toLowerCase())
                    })
                }
                else return this.options
            }

            else return this.options
        }
    },
    props:{
        name:{type:String},
        label:{type:String},
        errorText:{type:String},
        hasError:{type:Boolean},
        required:{type:Boolean},
        description:{type:String},
        value:{default:null},
        options:{type:Array},
        autocomplete:{type:Boolean},
        multiple:{type:Boolean,default:false}
    },

    watch:{
        hasError:function(val){
            if(val) this.inputVal=this.errorText
        },
    }
    ,
    methods:{
        closeList(){
            this.active=false
        },
        
        select(option){
            if(!this.multiple){
                this.selected=Object.assign({}, option)
                this.active=false
                this.inputVal=option 
            }

            else {
                if(!this.multipleSelected.some((element)=>{return element.value==option.value})){
                    this.multipleSelected.push(Object.assign({}, option))
                    this.inputVal=this.multipleSelected
                }
            }       
        },

        deleteSelect(option){
            this.multipleSelected=this.multipleSelected.filter((item)=>{
                return item.value!=option.value
            })
            this.inputVal=this.multipleSelected
        },

        checkIsSelect(value){
            return this.selected.value==value
        }
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

$errorColor:#eb5757;
$screenColor:white;
$laptopColor:#f2f2f2;
$labelColor:#749897;
$inputColor:#175351;
$focusColor:#007aff;
$focusColor1:#749897;
$badgeColor:#ffba51;

@mixin laptop {
  @media screen and (max-width: 960px) {
    @content;
  }
}



    .text-field{
        display:flex;
        flex-direction: column;
        align-items: flex-start;
        position:relative;
        width:100%;
        z-index:auto;

        &__dropDown{
            right:10px;
            position:absolute;
            fill:$inputColor;
            cursor:pointer;
            z-index:3;
            transition:transform 0.3s;
        }


       &__multiple{
           display:flex;
           position:absolute;
           overflow: auto;
           width:90%;
           z-index:998;
           &_item{
               z-index:999;
               display:flex;
               align-items:center;
               background:$labelColor;
               color:white;
               font-size:14px;
               padding:0px 8px;
               line-height: 20px;
               font-weight: 300;
               border-radius: 7px;
               margin-right:5px;
               margin-left:5px;
           }
           &_icon{
               display:flex;
               align-items: center;
               margin-left:5px;
               margin-bottom:2px;
               cursor:pointer;
           }
       }
    }

    .active{
        background-color:$focusColor1;
            .list__item_text{
                    color:white;
            }
    }

    .activeItem{
        background-color:$focusColor1;
        .list__item_text{
            color:white;
        }
    }

    .rotate180{
        transform:rotate(180deg);
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
        border:2px solid transparent;
        box-shadow: 0px 0px 10px 0 rgba(0, 0, 0, 0.05);
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

    .badge{
        color:white;
        background:$badgeColor;
        font-size:14px;
        padding:0px 8px;
        line-height: 20px;
        font-weight: 300;
        border-radius: 7px;
    }
</style>