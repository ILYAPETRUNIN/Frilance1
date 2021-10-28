<template>
  <div id="app">
      <div  class='container'>
          <TextField name='textField1' v-model='textField' required label='Текстовое поле'  description='Описание'/>
          <TextField name='textField2' v-model='textField' hasError errorText='Поле с ошибкой' required label='Текстовое поле'  description='Описание'/>
      </div>
      
      <div class='container'>
          <NumberField name='NumberField1' v-model='numberField' label='Числовое поле' description='Описание'/>
          <NumberField name='NumberField2' v-model='numberField' hasError errorText='Поле с ошибкой' label='Числовое поле' description='Описание'/>
      </div>

      <div class='container'>
          <List name='List1' hasError errorText='Поле с ошибкой' :options='optionsList' v-model='listField' label='Выпадающий список' description='Описание'/>
          <List name='List2' autocomplete :options='optionsList' v-model='listField' label='Выпадающий список autoComplete' description='Описание'/>
          <List name='List3' multiple :options='optionsList' v-model='listField' label='Выпадающий список multiple' description='Описание'/>
      </div>

      <div class='container'>
          <Switcher name='Switch1' :options='optionsSwitch' multiple v-model='switchField.multiple' label='Переключалка с multiple' description='Описание'/>
          <Switcher name='Switch2' :options='optionsSwitch' v-model='switchField.notMultiple' label='Переключалка без multiple' description='Описание'/>
          <Switcher name='Switch3' :options='optionsSwitch' v-model='switchField.error' label='Переключалка без multiple' description='Описание' hasError errorText='Поле с ошибкой'/>
      </div>

      <div class='checkBox'>
          <CheckBox required name='CheckBox1' label='Я чекбокc' v-model='checkboxField.one'/>
          <CheckBox name='CheckBox1' label='Я тоже чекбокc' v-model='checkboxField.two'/>
      </div>

      <div class='container'>
          <ButtonUI label='Я кнопка' name='Button1'/>
          <ButtonUI label='Я тоже' name='Button2'/>
      </div>
      
  </div>
</template>

<script>
import TextField from "./components/textField.vue";
import NumberField from "./components/numberField.vue";
import List from "./components/list.vue";
import Switcher from "./components/switch.vue";
import CheckBox from "./components/checkbox.vue";
import ButtonUI from "./components/button.vue";

let optionsList= [
          { value: 'sber', label: 'СБЕРБАНК',isOnline:true },
          { value: 'vtb', label: 'ВТБ' },
          { value: 'promTrans', label: 'ПРОМТРАНСБАНК',isOnline:true },
          { value: 'pochta', label: 'ПОЧТА БАНК',isOnline:true },
          { value: 'uralsib', label: 'УРАЛСИБ',isOnline:true },
        ]

let optionsSwitch=[
        { value: 'life', label: 'Жизнь'},
        { value: 'property', label: 'Имущество' },
        { value: 'dignity', label: 'Титул'},
]

export default {
  name: "App",
  data(){
    return{
      textField:null,
      numberField:undefined,
      listField:null,
      switchField:{
        multiple:[],
        notMultiple:[],
        error:[]
      },
      checkboxField:{
        one:true,
        two:false,
      },
      optionsList,
      optionsSwitch
    }
  },
  components: {
      TextField,
      NumberField,
      List,
      Switcher,
      CheckBox,
      ButtonUI
  },
};
</script>



<style lang="scss" scoped>
@mixin laptop {
  @media screen and (max-width: 960px) {
    @content;
  }
}

#app {
  font-family: Open\ Sans,Arial,Helvetica,sans-serif;
  background-color: #f2f2f2;
  padding:50px;
}
.container{
  padding:0px;
  margin:0px;
  display:flex;
  @include laptop{flex-wrap: wrap;};
  margin-bottom:50px;
}
.checkBox{
  display:flex;
  margin-bottom:50px;
  width:40%;
  flex-direction: column;
}
</style>
