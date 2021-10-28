<template>
        <FrameUI  v-bind="$props" :focused='focused'>
            <input :id='name' @focus="focused = true" @blur="focused = false" v-model='inputVal' class="text-field__input" type="text">
        </FrameUI>
</template>

<script>

import FrameUI from './frame.vue'

export default {
    components:{
        FrameUI
    },

    data(){
        return{
            focused:false,
            isShowDescription:false
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
        value:{type:String,default:null},
    },

}
</script>

