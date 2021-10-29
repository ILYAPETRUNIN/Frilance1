<template>
    <div class='body'>
        <label :class='{checkbox_active:inputVal}' class='checkbox' :for='name'>
            <input class='checkbox_input' v-model='inputVal' :id='name' type="checkbox">
            <div v-if='inputVal' class='checkbox_icon'>
                <svg xmlns="http://www.w3.org/2000/svg" fill="white" viewBox="0 0 50 50" width="14px" height="14px"><path d="M 41.9375 8.625 C 41.273438 8.648438 40.664063 9 40.3125 9.5625 L 21.5 38.34375 L 9.3125 27.8125 C 8.789063 27.269531 8.003906 27.066406 7.28125 27.292969 C 6.5625 27.515625 6.027344 28.125 5.902344 28.867188 C 5.777344 29.613281 6.078125 30.363281 6.6875 30.8125 L 20.625 42.875 C 21.0625 43.246094 21.640625 43.410156 22.207031 43.328125 C 22.777344 43.242188 23.28125 42.917969 23.59375 42.4375 L 43.6875 11.75 C 44.117188 11.121094 44.152344 10.308594 43.78125 9.644531 C 43.410156 8.984375 42.695313 8.589844 41.9375 8.625 Z"/></svg>
            </div>
        </label>
        <label class='label' :for='name'>{{label}}</label>
    </div>
</template>

<script>
export default {
    data(){
        return{
            
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
        value:{type:Boolean,default:false},
        label:{type:String}
    }
}
</script>

<style lang="scss" scoped>
        .body{
            display:flex;
            align-items:center;
            justify-content:space-between;
            width:100%;
        }
        .checkbox{
            margin-right:10px;
            display:flex;
            align-items:center;
            justify-content:center;
            cursor:pointer;
            width:16px;
            height:16px;
            background:transparent;
            z-index:2;
            border: 2px solid #175351;
            border-radius: 4px;
            transition:opacity 0.5, border .15s , background .15s , transform .15s;

            &:hover{
                border-color:#175351;
            }

            &_active{
                background-color:#175351;
            }

            &_input{
                z-index:-1;
                opacity: .001;
                outline:none;
            }
            &_icon{
                position:absolute;
                width:14px;
                height:14px;
            }
        }
        .label{
            font-size: 11px;
            line-height: 15px;
            color: #749897;
        }
</style>