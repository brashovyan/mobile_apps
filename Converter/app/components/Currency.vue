<template>
    <Page>
      <ActionBar>
        <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home" />
        <Label text="Валюта" class="action__label"/>
      </ActionBar>
  
      <FlexboxLayout class="flexbox__main">
        <FlexboxLayout class="flexbox__input">
          <TextField v-model="textFieldValue" keyboardType="number" maxLength="15" hint="Введите величину" class="textfield"/>
          <ListPicker :items="measure" v-model="selectedMeasure" class="selector"/>
        </FlexboxLayout>

        <template v-if="error == true">
          <Label text="Загрузка курсов валют!" class="error"/>
        </template>
  
        <ScrollView>
          <FlexboxLayout class="flexbox__output">
          <label :text="rub" class="label"/>
          <label :text="usd" class="label"/>
          <label :text="eur" class="label"/>
        </FlexboxLayout>
        </ScrollView> 
      </FlexboxLayout>
    </Page>
  </template>
  
  <script lang="ts">
  import { Http } from '@nativescript/core';
  import Vue from "nativescript-vue";
  import HomeVue from "./Home.vue";
  
  export default Vue.extend({
    data() {
        return {
          textFieldValue: "0",
          measure: [],
          selectedMeasure: "",
  
          rub: "",
          usd: "",
          eur: "",
          
          // реальные курсы валют с центрабанка рф на сегодня
          bank_usd: "",
          bank_eur: "",
          error: false,
        };
      },
  
    mounted() {
      Http.getJSON('https://www.cbr-xml-daily.ru/daily_json.js').then(
        (result: any) => {
          //console.log(result.Valute.USD.Value);
          this.bank_usd = result.Valute.USD.Value;
          this.bank_eur = result.Valute.EUR.Value;
          this.calculate();
        },
         e => {}
      );

        this.measure = ['₽', '$', '€'];
    },
  
    watch: { // при изменении вводимого значения вызывается этот метод
      textFieldValue: function(){
        this.calculate();
      },
  
      selectedMeasure: function(){
        this.calculate();
      },
    },
  
    methods: {
      home(){
        this.$navigateTo(HomeVue)
      },
  
      calculate(){
        // пока не загрулись данные с сайта, крутимся в рекурсии
        // единственное есть баг, что если мы зашли на эту страницу без интернета, а потом интернет включили
        // данные не загрузятся, т.к. мы только один раз обращаемся к API
        // пока лень с этим париться, а просто дедосить сайт каждую секунду сомнительно

        // по хорошему надо бы заносить последние данные о курсах в память телефона
        // чтоб если нет инета, просто показать старые данные

        if(this.bank_usd == ""){
          //setTimeout(()=> { this.calculate(); }, 1000);
          this.error = true;
        }
        else
        {
          this.error = false;

          if(this.textFieldValue != ""){ //если поле значения не пустое
            if(this.selectedMeasure == 0 || isNaN(this.selectedMeasure)){ // рубли
                this.rub = `₽: ${this.textFieldValue*1}`;
                this.usd = `$: ${(parseFloat(this.textFieldValue))/(parseFloat(this.bank_usd))}`
                this.eur = `€: ${(parseFloat(this.textFieldValue))/(parseFloat(this.bank_eur))}`
                //this.usd = `$: ${this.bank_usd}`
                //this.eur = `€: ${this.bank_eur}`
            }
            else if(this.selectedMeasure == 1){ // доллары
                this.rub = `₽: ${this.textFieldValue*parseFloat(this.bank_usd)}`;
                this.usd = `$: ${this.textFieldValue*1}`
                this.eur = `€: ${(this.textFieldValue*parseFloat(this.bank_usd))/parseFloat(this.bank_eur)}`
            }
            else if(this.selectedMeasure == 2){ // евро
                this.rub = `₽: ${this.textFieldValue*parseFloat(this.bank_eur)}`;
                this.usd = `$: ${(this.textFieldValue*parseFloat(this.bank_eur))/parseFloat(this.bank_usd)}`
                this.eur = `€: ${this.textFieldValue*1}`
            }
          }
          else{
              this.rub = `₽: 0`;
              this.usd = `$: 0`
              this.eur = `€: 0`
          }
        }
      },
    }
  });
  </script>
  
  <style scoped lang="scss">
  @import '@nativescript/theme/scss/variables/blue';
  
  page {
    background-color: rgb(199, 199, 199);  
  }
  
  actionbar {
    background-color: rgb(98, 98, 98);
  }
  
  .action__label {
    color: white;
  }
  
  .flexbox__main {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    height: auto;
    width: auto;
  }
  
  .flexbox__input{
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    flex-wrap: nowrap;
    margin-top: 30px;
    height: auto;
    width: auto;
  }
  
  .textfield {
    width: 70%;
    height: auto;
    background-color: white;
    color: black;
    font-size: 24px;
  }
  
  .selector {
    background-color: rgb(255, 255, 255);
    color: rgb(0, 0, 0);
    border-color: rgb(0, 0, 0);
    border-radius: 20;
    border-width: 2;
    width: 200px;
    height: 350px;
  }
  
  .flexbox__output {
    display: flex;
    flex-direction: column;
    height: auto;
    width: auto;
  }
  
  .label {
    color: black;
    font-size: 25px;
    margin-top: 30px;
    margin-left: 50px;
  }

  .error {
    font-size: 28px;
    color: red;
    margin-left: 30px;
  }
  </style>