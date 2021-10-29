<template>
    <button @click='click' :disabled='disabled' :class='{invert__button:invert,disabled:disabled}' class='button'>{{label}}</button>
</template>

<script>
export default {
    props:{
        name:{type:String},
        label:{type:String},
        disabled:{type:Boolean,default:false},
        invert:{type:Boolean,default:false}
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
    .button{
        cursor:pointer;
        outline:none;
        border:none;
        height:40px;
        width:100%;
        color: white;
        background-color: $inputColor;
        background-position: 50%;
        background-size: cover;
        border-radius: 10px;
        transition: .3s;
        font-family: inherit;
        font-size: 14px;
        text-transform: uppercase;
        text-decoration: none;
        font-weight: 600;
        &:hover{

            background-color: $buttonHover
        }
    }

    .invert{
        &__button{
            background-color: white;
            border:2px solid $focusColor;
            color: #c1cecb;
            &:hover{
               background-color: $focusColor;   
               color:white;        
            }
        }
    }

    .disabled{
        cursor:auto;
        background-color: #c1cecb; 
        &:hover{
            background-color: #c1cecb;   
            color:white;    
        }
    }
</style>