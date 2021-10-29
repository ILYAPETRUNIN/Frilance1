<template>
    <div class='promoCode'>
        <p v-if='errorText!=""' class='promoCode__error'>Промокод не действителен</p>
        <div class='promoCode__body'>
            <TextField :hasError='errorText!=""'  :name='name+"_promoCodeText"' placeholder='Введите промокод'/>
            <ButtonUI class='promoCode__button' invert :name='name+"_promoCodeButton"' :label='(!isApplyed) ? "Применить" :"Отменить"'/>
        </div>      
    </div>
</template>

<script>
import TextField from './textField.vue'
import ButtonUI from './button.vue'

export default {
    components:{
        TextField,
        ButtonUI
    },
    props:{
        promocode:{type:String},
        name:{type:String},
        label:{type:String},
        errorText:{type:String,default:""},
        isApplyed:{type:Boolean,default:false}
    },

    methods:{
        click(){
            this.$emit('click')
        }
    }
}
</script>

<style lang="scss" scoped>
@import '@/scss/main.scss';

        .promoCode{
            width:100%;
            &__body{
                display:flex;
                align-items: center;
                justify-content:space-between;
            }

            &__button{
                margin-top:5px;
                margin-left:10px;
            }
            &__error{
                position:absolute;
                margin-top:-5px;
                font-size: 11px;
                color:$errorColor;
            }
        }
    
</style>