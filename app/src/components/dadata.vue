<template>
          <transition name="fade"> 
                <div v-if='isShow' class=list>
                    <ul class=list__body>
                        <li  @click='suggestSelect(suggest)' class="list__item" v-for='(suggest,index) in suggests' :key='index'>
                                    <p class='list__item_text'>{{suggest.value}}</p>
                        </li>
                        <p class="list__item_not" v-if='suggests.length==0'>Не найдено совпадений</p>
                    </ul>
                </div> 
         </transition>
</template>

<script>

export default {
    data(){
        return{
            suggests:[]
        }
    },

    props:{
        isShow:{type:Boolean,default:false},
        text:{type:String},
        type:{type:String,default:'adress'},
        token:{type:String,default:""},
        showItems:{type:Number,default:3}
    },

    methods:{
            async getData(data){
                var url = "https://suggestions.dadata.ru/suggestions/api/4_1/rs/suggest/"+this.type;

                var options = {
                    method: "POST",
                    mode: "cors",
                    headers: {
                        "Content-Type": "application/json",
                        "Accept": "application/json",
                        "Authorization": "Token " + this.token
                    },
                    body: JSON.stringify({query: data})
                }
                try{
                    let response = await fetch(url, options);
                    let data=await response.json()
                    this.suggests=data.suggestions.slice(0,this.showItems)
                }
                catch(e){
                    console.log(e)
                    throw e
                }
            },

            suggestSelect(suggest){
                this.$emit('suggestSelect',suggest)
            }
    },
    watch:{
        text:function(val){
            this.getData(val)
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
</style>

<style lang="scss" scoped>
@import '@/scss/main.scss';

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
        left:-2px;
        border:2px solid transparent;
        border-radius: 10px;
        box-shadow: 0px 5px 10px 0 rgba(0, 0, 0, 0.05);
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
</style>