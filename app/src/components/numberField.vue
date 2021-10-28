<template>
        <FrameUI  v-bind="$props" :focused='focused'>
            <input :id='name' v-on:keypress='onlyNumber'  @focus="focused = true" @blur="focused = false" v-model='inputVal' class="text-field__input" type="text">
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
                if(this.value.includes(","))    return this.value
                else return this.toLocaleNumber(this.value).toLocaleString('ru-RU')
            },
            set(val){
                if(val.includes(","))    this.$emit('input',val)
                else  this.$emit('input',this.toLocaleNumber(val).toLocaleString('ru-RU'))           
            }
        },

        numValue(){
            return this.toLocaleNumber(this.inputVal)
        }
    },

    props:{
        name:{type:String},
        label:{type:String},
        errorText:{type:String},
        hasError:{type:Boolean},
        required:{type:Boolean},
        description:{type:String},
        value:{type:String,default:''},
    },

    watch:{
        hasError:function(val){
            if(val) this.inputVal=this.errorText
        }
    }
    ,
    methods:{
        clickAway(){
            this.isShowDescription=false
        },

        onlyNumber(event){
            if(/^[0-9]$/.test(event.key)){
                if((this.inputVal.split(',')[1] || '').length < 2) return;
                event.preventDefault();
            }
            else if(/^[.,]$/.test(event.key)){
                event.preventDefault();
                if(this.inputVal.includes(",")) return;
                this.inputVal=this.inputVal+","            
            }
            else event.preventDefault();          
        }, 

        toLocaleNumber(val){
            let result=val.replace(/\s/g, '');
            result=result.replace(',','.')
            return Number(result)
        }
    }
}
</script>

