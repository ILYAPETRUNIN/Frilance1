<template>
    <div class='adressField'>
        <div class='adressField__inputs'>
            <FrameUI class='adressField__input1' :name='name+"_frame1"' :description='!freeForm ? description:null' :errorText='errorText' :hasError='hasError' :required='required'  v-bind="$props" :focused='focused'>
                <input :id='name+"_input1"' @focus="focused = true" @blur="focused = false" v-model='input1' class="text-field__input" type="text">
                <dadata :token='token' v-if='!freeForm' @suggestSelect='suggestSelect' :showItems='5' :isShow='focused' :text='input1' type='address' />
            </FrameUI>
            
            <FrameUI v-if='freeForm' class='adressField__input2' label='№ квартиры'   :name='name+"_frame2"' :description='description' :focused='focused' :hasError='hasError'  :required='required'>
                <input :id='name+"_input2"' @focus="focused = true" @blur="focused = false" v-model='input2' class="text-field__input" type="text">
            </FrameUI>
        </div>

        <CheckBox class='adressField__checkbox' v-model='freeForm' label='Ввести адресс в свободной форме'   :name='name+"_checkBox"'/>
        

    </div>
</template>

<script>

import FrameUI from './frame.vue'
import CheckBox from './checkbox.vue'
import dadata from './dadata.vue'

export default {
    components:{
        FrameUI,
        CheckBox,
        dadata
    },

    data(){
        return{
            focused:false,
            isShowDescription:false,
            freeForm:false,
            input1:'',
            input2:'',
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
        input1:function(val){
            if(!this.selectDadata){
                delete this.inputVal.valueAddress
                this.inputVal.valueText=val
            }
            else{this.selectDadata=false}
        },
        input2:function(val){
            this.inputVal.valueFlat=val
        },
        freeForm:function(val){
            this.inputVal.isText=val
            this.input1=''
            this.input2=''

            if(!val){
                delete this.inputVal.valueFlat
                delete this.inputVal.valueAddress
            }

        }
    },
    methods:{
        suggestSelect(suggest){
            if(suggest.data.city) this.fullInfo.city=suggest.data.city
            if(suggest.data.street) this.fullInfo.street=suggest.data.street
            if(suggest.data.house) this.fullInfo.house=suggest.data.house
            if(suggest.data.block) this.fullInfo.building=suggest.data.block
            if(suggest.data.flat) this.fullInfo.flat=suggest.data.flat
            if(suggest.data.fias_id) this.fullInfo.fiasId=suggest.data.fias_id
            if(suggest.data.city_fias_id) this.fullInfo.cityFiasId=suggest.data.city_fias_id

            this.inputVal.valueText=suggest.value
            this.inputVal.valueAddress=this.fullInfo

            this.input1=suggest.value
            this.selectDadata=true
        }   
    }

}
</script>



<style lang="scss" scoped>
@import '@/scss/main.scss';
    .adressField{
        width:100%;
        &__inputs{
            display:flex;
            width:100%;
        }
        &__input1{
            width:70%;
            margin-right:20px;
        }
        &__input2{
            width:30%;
        }
        &__checkbox{
            max-width:210px;
            margin-top:30px;
        }
    }

</style>