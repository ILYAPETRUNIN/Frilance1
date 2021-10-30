<template>
            <FrameUI class='nameField__input' :name='name+"_frame"' :description='!freeForm ? description:null' :errorText='errorText' :hasError='hasError' :required='required'  v-bind="$props" :focused='focused'>
                <input :id='name+"_input"' @focus="focused = true" @blur="focused = false" v-model='input' class="text-field__input" type="text">
                <dadata  @suggestSelect='suggestSelect' :showItems='5' :isShow='focused' :text='input' type='fio' :token = 'token'/>
            </FrameUI>
</template>

<script>

import FrameUI from './frame.vue'
import dadata from './dadata.vue'

export default {
    components:{
        FrameUI,
        dadata
    },

    data(){
        return{
            focused:false,
            isShowDescription:false,
            freeForm:false,
            input:'',
            selectDadata:false,
            fullInfo:{},
            isShowDadata:false
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
        value:{default:{}},
        token:{default:String}
    },
    watch:{
        input:function(val){
            if(!this.selectDadata){
                delete this.inputVal.valueFio
                this.inputVal.valueText=val
            }
            else{this.selectDadata=false}
        },
    },

    methods:{
        suggestSelect(suggest){
            console.log(suggest)
            if(suggest.data.name) this.fullInfo.firstName=suggest.data.name
            if(suggest.data.surname) this.fullInfo.lastName=suggest.data.surname
            if(suggest.data.patronymic) this.fullInfo.middleName=suggest.data.patronymic

            this.inputVal.valueText=suggest.value
            this.inputVal.valueFio=this.fullInfo

            this.input=suggest.value
            this.selectDadata=true
        }   
    }

}
</script>



<style lang="scss" scoped>
@import '@/scss/main.scss';
    .nameField{
        &__input{
            width:100%;
        }
    }
    

</style>