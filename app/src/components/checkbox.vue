<template>
  <div class='v-checkbox'>
      <div class='text-field__label'>
            <label class='checkbox__label' for="name">{{label}}</label>
            <span class="text-field__label_required" v-if='required'>*</span>
      </div>
      <ToggleButton v-model='inputVal' id='name' :margin='0'  :width='46' css-colors />
  </div>
</template>

<script>

import { ToggleButton } from 'vue-js-toggle-button'

export default {
    components:{
        ToggleButton
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
    },
    props:{
        name:{type:String},
        label:{type:String},
        required:{type:Boolean},
        value:{default:null},
    },

}
</script>

<style lang="scss" >
$errorColor:#eb5757;
$screenColor:white;
$laptopColor:#f2f2f2;
$labelColor:#749897;
$inputColor:#175351;
$focusColor:#007aff;

@mixin laptop {
  @media screen and (max-width: 960px) {
    @content;
  }
}
    .v-checkbox{
        display:flex;
        align-items:center;
        width:100%;
        justify-content:space-between;

        &__label{
            font-size: 14px;
            line-height: 20px;
            margin-right: 5px;
        }
    }
    .text-field{

        &__label{
            &_required{
                margin-left:5px;
                 color:$errorColor;
            }
        }
    }
    .v-switch{
                &-core{      
                    background: white;
                    @include laptop{background:#f2f2f2};
                    border-radius: 34px;
                }
                &-button{
                    background: #c1cecb!important;
                }
        }

    .toggled{
        .v-switch{
                &-core{      
                    background: linear-gradient(90deg,#43706e 1%,#749897 63%);
                    border-radius: 34px;
                }
                &-button{
                    background-color: #175351!important;
                }
        }
    }
</style>