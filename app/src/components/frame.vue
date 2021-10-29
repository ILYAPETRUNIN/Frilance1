<template>
        <div class='text-field'>
            <label class="text-field__label" :for="name">
                {{label}}
                <span class="text-field__label_required" v-if='required'>*</span>
            </label>
            <div class="text-field__field">
                <div :class='{focus:focused,error:hasError,transparent:transparent}' class='text-field__inner'>
                    <p class='text-field__error' v-if='hasError'>{{errorText}}</p>
                    <slot></slot>
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

import Vue from 'vue'
import VTooltip from 'v-tooltip'

Vue.use(VTooltip)

export default {
    data(){
        return{
            isShowDescription:false
        }
    },

    props:{
        name:{type:String},
        label:{type:String},
        errorText:{type:String},
        hasError:{type:Boolean},
        required:{type:Boolean},
        description:{type:String},
        focused:{type:Boolean,default:false},
        transparent:{type:Boolean,default:false}
    },

    methods:{
        clickAway(){
            this.isShowDescription=false
        }
    }
}
</script>

<style lang="scss">
@import '@/scss/main.scss';

.tooltip {
  display: block !important;
  z-index: 10000;
  font-family: Open\ Sans,Arial,Helvetica,sans-serif;
  font-size:11px;

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
.text-field__input{
            width:100%;
            outline:none;
            border:none;
            background:transparent;
            padding:15px;
            z-index:2;
            color: $inputColor;
            font-size: 14px;
        }
</style>

<style lang="scss" scoped>

@import '@/scss/main.scss';


    .text-field{
        display:flex;
        flex-direction: column;
        align-items: flex-start;
        width:100%;
        z-index:1;

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
            width:100%;
            background:$screenColor;
            align-items:center;
            @include laptop{background:$laptopColor};
            height:50px;
            border-radius: 10px;
            border:2px solid transparent;
            box-shadow: 0px 0px 10px 0 rgba(0, 0, 0, 0.05);
            transition: all .3s;
            position: relative;
        }

        &__error{
            position:absolute;
            top:-7px;
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
    }

    .focus{
        border:2px solid $focusColor;
    }
    .error{
        border:2px solid $errorColor;
    }

    .transparent{
      background-color:transparent!important;
      box-shadow: none!important;
    }
</style>