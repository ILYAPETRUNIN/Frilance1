<template>

            <FrameUI :name='name' :label='label' :required='required' :description='description' transparent>
                <div class='switch__body'>
                    <div class='switch__inputsList'>
                        <label :class='{active:isActive(option.value),error:hasError,hoverable:!isActive(option.value)}' class="switch__item" :for='name+option.value' v-for='option in options' :key='option.value'>
                                {{option.label}}
                                <input class='switch__input' :id='name+option.value' :value='option.value'  v-if='multiple' type="checkbox" v-model='inputVal'/>
                                <input class='switch__input' :id='name+option.value' :value='option.value' v-else type="radio"    v-model='inputVal'/>
                        </label>
                    </div>

                    <p class='switch__error' v-if='hasError'>{{errorText}}</p>
                </div>
            </FrameUI>
</template>

<script>
import FrameUI from './frame.vue'

export default {
    components:{
        FrameUI
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
        errorText:{type:String},
        hasError:{type:Boolean},
        required:{type:Boolean},
        description:{type:String},
        value:{default:null},
        options:{type:Array},
        multiple:{type:Boolean,default:false}
    },
    methods:{
        isActive(val){
            return this.inputVal.includes(val)
        }  
    }
}
</script>



<style lang="scss" scoped>
@mixin laptop {
  @media screen and (max-width: 960px) {
    @content;
  }
}

$errorColor:#eb5757;
$screenColor:white;
$laptopColor:#f2f2f2;
$labelColor:#749897;
$inputColor:#175351;
$focusColor:#007aff;

.switch{
        display:flex;
        flex-direction: column;
        align-items: flex-start;
        width:100%;
        z-index:1;



        &__item{
            display:flex;
            align-items:center;
            justify-content: center;
            cursor:pointer;
            margin:0px 1px;
            width:100%;
            height:50px;
            border:2px solid transparent;
            background:white;
            @include laptop{background:$laptopColor};
            &:first-child{
                border-radius: 10px 0px 0px 10px;
            }
            &:last-child{
                border-radius: 0px 10px 10px 0px;
            }
        }
        &__input{
            width:1px;
            position:absolute;
            z-index:-1;
        }

        &__error{
            position: absolute;
            margin-bottom:5px;
            font-size: 11px;
            color:$errorColor;
        }

        &__body{
          position:relative; 
          width:100%;
        }

        &__inputsList{
          display:flex;
          width:100%;
          align-items: center;
          justify-content: center;
        }
}

        .active{
            color: #fff;
            background: $inputColor;
            position:relative;

            &::before{
                border-radius: 10px;
                content: "";
                position: absolute;
                top: 0;
                left: 0;
                right: 0;
                bottom: 0;
                z-index: 0;
                box-shadow: inset -10px 0 15px rgb(0 0 0 / 15%), inset 10px 0 15px rgb(0 0 0 / 15%);
            }
        }

        .error{
          border:2px solid $errorColor;
        }

        .hoverable{
                    &:hover{
                background-color:#e6e9e9;
            }
          }
</style>

