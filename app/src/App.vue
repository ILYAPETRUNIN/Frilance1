<template>
  <div id="app">
      <div  class='container'>
          <TextField :style="{marginRight:'10px'}" name='textField1' v-model='textField' required label='Текстовое поле'  description='Описание'/>
          <TextField name='textField2' v-model='textField' hasError errorText='Поле с ошибкой' required label='Текстовое поле'  description='Описание'/>
      </div>
      
      <div class='container'>
          <NumberField :style="{marginRight:'10px'}" name='NumberField1' v-model='numberField' label='Числовое поле' description='Описание'/>
          <NumberField name='NumberField2' v-model='numberField' hasError errorText='Поле с ошибкой' label='Числовое поле' description='Описание'/>
      </div>

      <div class='container'>
          <List :style="{marginRight:'10px'}" name='List1' hasError errorText='Поле с ошибкой' :options='optionsList' v-model='listField' label='Выпадающий список' description='Описание'/>
          <List :style="{marginRight:'10px'}" name='List2' autocomplete :options='optionsList' v-model='listField' label='Выпадающий список autoComplete' description='Описание'/>
          <List :style="{marginRight:'10px'}" name='List3' multiple :options='optionsList' v-model='listField' label='Выпадающий список multiple' description='Описание'/>
      </div>

      <div class='container'>
          <Switcher :style="{marginRight:'10px'}" name='Switch1' :options='optionsSwitch' multiple v-model='switchField.multiple' label='Переключалка с multiple' description='Описание'/>
          <Switcher :style="{marginRight:'10px'}" name='Switch2' :options='optionsSwitch' v-model='switchField.notMultiple' label='Переключалка без multiple' description='Описание'/>
          <Switcher name='Switch3' :options='optionsSwitch' v-model='switchField.error' label='Переключалка без multiple' description='Описание' hasError errorText='Поле с ошибкой'/>
      </div>

      <div class='checkBox'>
          <CheckBox :style="{marginBottom:'10px'}" required name='CheckBox1' label='Я чекбокc' v-model='checkboxField.one'/>
          <CheckBox :style="{marginBottom:'10px'}" name='CheckBox1' label='Я тоже чекбокc' v-model='checkboxField.two'/>
      </div>

      <div class='container'>
          <ButtonUI :style="{marginRight:'10px'}" label='Я кнопка' name='Button1'/>
          <ButtonUI :style="{marginRight:'10px'}" label='Я тоже' invert name='Button2'/>
          <ButtonUI label='Я отключена' disabled name='Button3'/>
      </div>

      <div :style="{marginBottom:'50px'}">
          <PromoCode :style="{marginBottom:'10px'}" name='promoCode1' />
          <PromoCode :style="{marginBottom:'10px'}" name='promoCode2' isApplyed/>
          <PromoCode name='promoCode3' errorText='Промокод не действителен'/>
      </div>

      <div class='container'>
          <AdressField :style="{marginRight:'10px'}"  v-model='adressField' name='addresField1' label='Адрес объекта' description="Описание адреса"/>
          <AdressField v-model='adressField1' name='addresField2' label='Адрес объекта' description="Описание адреса" required hasError errorText='Ошибка'/>
      </div>
      
  </div>
</template>

<script>
import TextField from "./components/textField.vue";
import NumberField from "./components/numberField.vue";
import List from "./components/list.vue";
import Switcher from "./components/switch.vue";
import CheckBox from "./components/toogleButton.vue";
import ButtonUI from "./components/button.vue";
import PromoCode from "./components/PromoCode.vue";
import AdressField from "./components/adressField.vue";

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
      adressField:{},
      adressField1:{},
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
      ButtonUI,
      PromoCode,
      AdressField
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
