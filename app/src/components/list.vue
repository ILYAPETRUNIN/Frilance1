<template>
        <div class='text-field'>
            <label class="text-field__label" :for="name">{{label}}
                <span class="text-field__label_required" v-if='required'>*</span>
            </label>
            <div class="text-field__field">
                <div :class='{error:hasError}' class='text-field__inner'>
                    <p class='text-field__error' v-if='hasError'>{{errorText}}</p>
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
                </div>
                <div v-if='description' class='text-field__description'>
    
                <v-popover placement='top' offset="20">
                    <div  @click='isShowDescription=true' class='text-field__description_icon'>
                        <svg xmlns="http://www.w3.org/2000/svg" width='20px' height='20px'>
                            <path d="M7.65 14.6h1.7v-1.7h-1.7v1.7zM8.5 1A8.503 8.503 0 000 9.5C0 14.192 3.808 18 8.5 18S17 14.192 17 9.5 13.192 1 8.5 1zm0 15.3a6.809 6.809 0 01-6.8-6.8c0-3.748 3.051-6.8 6.8-6.8 3.748 0 6.8 3.051 6.8 6.8 0 3.748-3.052 6.8-6.8 6.8zm0-11.9a3.4 3.4 0 00-3.4 3.4h1.7c0-.935.765-1.7 1.7-1.7.935 0 1.7.765 1.7 1.7 0 1.7-2.55 1.487-2.55 4.25h1.7c0-1.913 2.55-2.125 2.55-4.25a3.4 3.4 0 00-3.4-3.4z"></path>
                        </svg>
                    </div>

                    <template slot="popover">
                        {{description}}
                    </template>
                      
                </v-popover>
                </div>
            </div>



        </div>
</template>

<script>


import { directive as onClickaway } from 'vue-clickaway';
import Vue from 'vue'
import VTooltip from 'v-tooltip'

Vue.use(VTooltip)

export default {
    directives: {
         onClickaway: onClickaway,
    },

    components:{
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

    .tooltip {
  display: block !important;
  z-index: 10000;

  .tooltip-inner {
    background: white;
    color: #175351;
    padding:10px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px 0 rgba(0, 0, 0, 0.5);
  }

  .tooltip-arrow {
    width: 0;
    height: 0;
    border-style: solid;
    position: absolute;
    margin: 5px;
    border-color: white;
    z-index: 1;
  }

  &[x-placement^="top"] {
    margin-bottom: 5px;

    .tooltip-arrow {
      border-width: 5px 5px 0 5px;
      border-left-color: transparent !important;
      border-right-color: transparent !important;
      border-bottom-color: transparent !important;
      bottom: -5px;
      left: calc(50% - 5px);
      margin-top: 0;
      margin-bottom: 0;
    }
  }

  &[x-placement^="bottom"] {
    margin-top: 5px;

    .tooltip-arrow {
      border-width: 0 5px 5px 5px;
      border-left-color: transparent !important;
      border-right-color: transparent !important;
      border-top-color: transparent !important;
      top: -5px;
      left: calc(50% - 5px);
      margin-top: 0;
      margin-bottom: 0;
    }
  }

  &[x-placement^="right"] {
    margin-left: 5px;

    .tooltip-arrow {
      border-width: 5px 5px 5px 0;
      border-left-color: transparent !important;
      border-top-color: transparent !important;
      border-bottom-color: transparent !important;
      left: -5px;
      top: calc(50% - 5px);
      margin-left: 0;
      margin-right: 0;
    }
  }

  &[x-placement^="left"] {
    margin-right: 5px;

    .tooltip-arrow {
      border-width: 5px 0 5px 5px;
      border-top-color: transparent !important;
      border-right-color: transparent !important;
      border-bottom-color: transparent !important;
      right: -5px;
      top: calc(50% - 5px);
      margin-left: 0;
      margin-right: 0;
    }
  }

  &[aria-hidden='true'] {
    visibility: hidden;
    opacity: 0;
    transition: opacity .15s, visibility .15s;
  }

  &[aria-hidden='false'] {
    visibility: visible;
    opacity: 1;
    transition: opacity .15s;
  }

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

        &__label{
            padding: 0;
            margin: 0 0 10px;
            font-size: 11px;
            line-height: 15px;
            color: inherit;
            white-space: nowrap;
            color: $labelColor;
            &_required{
                 color:$errorColor;
            }
        }

        &__field{
            display:flex;
            width:100%;
            align-items:center;
        }

        &__inner{
            display:flex;
            align-items:center;
            width:100%;
            background:$screenColor;
            @include laptop{background:$laptopColor};
            height:50px;
            border-radius: 10px;
            border:2px solid transparent;
            box-shadow: 0px 0px 10px 0 rgba(0, 0, 0, 0.05);
            transition: all .3s;
            position: relative;
        }

        &__input{
            cursor:pointer;
            width:100%;
            outline:none;
            border:none;
            background:transparent;
            padding:15px;
            z-index:2;
            color: $inputColor;
            font-size: 14px;
        }

        &__dropDown{
            right:10px;
            position:absolute;
            fill:$inputColor;
            cursor:pointer;
            z-index:3;
            transition:transform 0.3s;
        }

        &__error{
            position:absolute;
            margin-top:-10px;
            margin-left:15px;
            font-size: 11px;
            color:$errorColor;
        }

        &__description{
            &_icon{
                display:flex;
                fill:$labelColor;
                margin-left:10px;
                cursor:pointer;
            }
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

    .focus{
        border:2px solid $focusColor;
    }
    .error{
        border:2px solid $errorColor;
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