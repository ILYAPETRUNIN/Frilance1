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

      <div :style="{marginBottom:'50px',padding:'20px',border:'1px solid black'}">
          <PromoCode :style="{marginBottom:'10px'}" name='promoCode1' />
          <PromoCode :style="{marginBottom:'10px'}" name='promoCode2' isApplyed/>
          <PromoCode name='promoCode3' errorText='Промокод не действителен'/>
      </div>

      <div class='container'>
          <AdressField token='312eddceaa8cc4208e30c15ad2e402392e275a8e' :style="{marginRight:'10px'}"  v-model='adressField' name='addresField1' label='Адрес объекта' description="Описание адреса"/>
          <AdressField token='312eddceaa8cc4208e30c15ad2e402392e275a8e' v-model='adressField1' name='addresField2' label='Адрес объекта' description="Описание адреса" required hasError errorText='Ошибка'/>
      </div>

       <div class='container'>
          <NameField token='312eddceaa8cc4208e30c15ad2e402392e275a8e' v-model='nameField1' name='nameField1' label='Фамилия Имя Отчество' description="Описание ФИО"/>
           <NameField token='312eddceaa8cc4208e30c15ad2e402392e275a8e' v-model='nameField2' name='nameField2' label='Фамилия Имя Отчество' description="Описание ФИО" required hasError errorText='Ошибка'/>
       </div>

       <div class='container'>
            <DateField name='dateField1' v-model='dateField1' label='Дата рождения' description="Описание даты"/>
            <DateField name='dateField2' v-model='dateField2' label='Дата рождения' description="Описание ФИО" required hasError errorText='Ошибка'/>
        </div>
      
        <div class='basket'>
            <Basket :summ='Number(diagram.summ)' :items='diagram.array'/>
            <input v-model='diagram.summ'  type="number">
            <button @click='addDiarammArray'>Добавить компонент</button>
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
import NameField from "./components/nameField.vue";
import DateField from "./components/dateField.vue";
import Basket from "./components/basket.vue";

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

let price=[
  {label:'Я label 1',price:'100',type:'main'},
  {label:'Я label 2',price:'50',type:'addition'},
  {label:'Я label 3',price:'25',type:'addition'},
  {label:'Я label 4',price:'12.5',type:'main'},
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
      nameField1:{},
      nameField2:{},
      dateField1:new Date(),
      dateField2:new Date(),
      diagram:{
        summ:0,
        array:[],
        count:0
      },

      optionsList,
      optionsSwitch,
      price
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
      AdressField,
      NameField,
      DateField,
      Basket
  },

  methods:{
    addDiarammArray(){
      if(this.diagram.count==this.price.length){
        this.diagram.count=0
        this.diagram.array=[]
      }
      this.diagram.array.push(this.price[this.diagram.count])
      this.diagram.count++
    }
  }
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
  @include laptop{padding:0px;};
}
.container{
  padding:0px;
  margin:0px;
  display:flex;
  @include laptop{flex-wrap: wrap;};
  margin-bottom:100px;

}
.checkBox{
  display:flex;
  margin-bottom:100px;
  width:40%;
  flex-direction: column;
  padding:20px;
  border:1px solid black;
}
.basket{
  width:400px;
}
</style>
