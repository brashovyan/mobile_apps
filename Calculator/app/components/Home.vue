<template>
  <Page class="page">
    <ActionBar class="header">
      <Label text="Калькулятор"/>
    </ActionBar>

    <GridLayout columns="*, *, *, *" rows="*, *, *, *, *, *, *" class="grid">
        <label :text="inputField" row="0" col="0" colSpan="4" class="result"/>
        <label :text="result" row="1" col="0" colSpan="4" class="result"/>

        <button text="C" @tap="clearAll()" row="2" col="0" colSpan="2" class="btnC"/>
        <button text="⌫" @tap="clearOne()" row="2" col="2" class="btnC"/>
        <button text="÷" @tap="input('÷')" row="2" col="3" class="btnAction"/>

        <button text="7" @tap="input('7')" row="3" col="0" class="btn"/>
        <button text="8" @tap="input('8')" row="3" col="1" class="btn"/>
        <button text="9" @tap="input('9')" row="3" col="2" class="btn"/>
        <button text="×" @tap="input('×')" row="3" col="3" class="btnAction"/>

        <button text="4" @tap="input('4')" row="4" col="0" class="btn"/>
        <button text="5" @tap="input('5')" row="4" col="1" class="btn"/>
        <button text="6" @tap="input('6')" row="4" col="2" class="btn"/>
        <button text="-" @tap="input('-')" row="4" col="3" class="btnAction"/>

        <button text="1" @tap="input('1')" row="5" col="0" class="btn"/>
        <button text="2" @tap="input('2')" row="5" col="1" class="btn"/>
        <button text="3" @tap="input('3')" row="5" col="2" class="btn"/>
        <button text="+" @tap="input('+')" row="5" col="3" class="btnAction"/>

        <button text="0" @tap="input('0')" row="6" col="0" colSpan="2" class="btn0"/>
        <button text="." @tap="inputDot()" row="6" col="2" class="btn"/>
        <button text="=" @tap="evenly()" row="6" col="3" class="btnAction"/>
    </GridLayout>
  </Page>
</template>

<script lang="ts">
import Vue from "nativescript-vue";

  export default Vue.extend({
    data() {
      return {
        result: "",
        inputField: "", // то, что отображается на калькуляторе (12 символов)
        realField: "", // реальное поле 
      };
    },

    methods: {
      input(x: string) { // ввод символа в поле
        if(x != '+' && x!= '-' && x != '×' && x != '÷'){ // если это не знак операции   
          if(this.inputField.length < 12){
            this.inputField += x;
            this.realField += x;
          } // если поле переполнилось
          else{
            this.realField += x;
            this.inputField = this.inputField.substring(1); // удаляю первый символ
            this.inputField += x; // вставляю в конец новый
          }
          this.calculate();
        }

        else if( x != '-'){ // если это не минус
          if(this.realField[this.realField.length - 1] != '+' && this.realField[this.realField.length - 1] != '-' && this.realField[this.realField.length - 1] != '×' && this.realField[this.realField.length - 1] != '÷'){
            // если последний символ не знак операции
            if(this.inputField.length < 12){
              this.inputField += x;
              this.realField += x;
            } // если поле переполнилось
            else{
              this.realField += x;
              this.inputField = this.inputField.substring(1); // удаляю первый символ
              this.inputField += x; // вставляю в конец новый
            }
              this.calculate();
          } // если последний символ - это знак операции, то заменяем его
          else{
            this.clearOne();
            this.input(x);
          }
        }

        else if(x == '-'){ // если мы ввели минус
          // если последний символ НЕ операция
          if(this.realField[this.realField.length - 1] != '+' && this.realField[this.realField.length - 1] != '-' && this.realField[this.realField.length - 1] != '×' && this.realField[this.realField.length - 1] != '÷'){
          
            if(this.inputField.length < 12){
              this.inputField += x;
              this.realField += x;
            } // если поле переполнилось
            else{
              this.realField += x;
              this.inputField = this.inputField.substring(1); // удаляю первый символ
              this.inputField += x; // вставляю в конец новый
            }
              this.calculate();
          }
          // если последний символ это - или +
          else if(this.realField[this.realField.length - 1] == '+' || this.realField[this.realField.length - 1] == '-'){
            // просто его заменяю
            this.clearOne();
            this.input(x);
          } 
          // если последний символ / или *,
          else if(this.realField[this.realField.length - 1] == '×' || this.realField[this.realField.length - 1] == '÷'){
            if(this.inputField.length < 12){
              this.inputField += x;
              this.realField += x;
            } // если поле переполнилось
            else{
              this.realField += x;
              this.inputField = this.inputField.substring(1); // удаляю первый символ
              this.inputField += x; // вставляю в конец новый
            }
            this.calculate();
          }
        }
      },

      inputDot() { // отдельный ввод точки (хотя это можно запихать в input)
        // если коротко, то сперва я проверяю, что последний символ - это цифра
        // дальше я паршу всё текстовое поле и нахожу последнее число (например 25.3)
        // если точка в числе уже есть - то нельзя
        // если точки в числе нет - можно поставить
        var previous = this.realField[this.realField.length - 1];
        if(previous == '0' || previous == '1' || previous == '2' || previous == '3' || previous == '4' || previous == '5' || previous == '6' || previous == '7' || previous == '8' || previous == '9'){
          var real = this.realField;
          var numbers = real.toString().split('÷');
          numbers = numbers.toString().split('×');
          numbers = numbers.toString().split('+');
          numbers = numbers.toString().split('-');
          numbers = numbers.toString().split(',');
          var num = numbers[numbers.length - 1];
          if(num.indexOf('.') == -1){
            this.input('.');    
          }       
        }
      },

      clearAll(){ // кнопка С отчистить всё
        this.inputField = "";
        this.realField = "";
        this.result= "";
      },

      clearOne(){ // стереть один символ
        //this.inputField = this.inputField.substring(0, this.inputField.length - 1);
        this.realField = this.realField.substring(0, this.realField.length - 1);
        if(this.realField.length > 12){
          this.inputField = this.realField.slice(-12);
        }
        else{
          this.inputField = this.realField;
        }
        this.calculate();
      },

      calculate(){ // сосчитать
        try{
          var res = this.realField; // заменяю символы, чтоб их понял js
          // из-за того, что js replace заменяет только первый найденный символ, пришлось пихать в цикл
          for(var i = 0; i < res.length; i++){
            res = res.replace('÷', '/');
            res = res.replace('×', '*');
          }

          var r = eval(res); //если результат целочисленный, то вывожу целое число
          if(r.toString().indexOf('.') == -1){
            this.result = r;    
          }
          else{ // если результат десятичный - округляю до двух знаков после запятой
            this.result = r.toFixed(2);
          }

          // если в результате счета получаем "бесконечность", пишу "ошибка"
          if(this.result == "Infinity" || this.result == "-Infinity"){
            this.result = "Ошибка";
          }
        }
        // если результата в принципе нет (т.е. не смог посчитать), то ничего
        catch{
          this.result = "";
        }
      },

      evenly(){ // кнопка =
        this.calculate();
        this.inputField = this.result.toString();
        this.realField = this.result.toString();
        this.result = "";
      }
    },
});
</script>

<style scoped lang="scss">
  @import '@nativescript/theme/scss/variables/blue';

  // Custom styles
  /*.fas {
    @include colorize($color: accent);
  }

  .info {
    font-size: 20;
    //horizontal-align: center;
    //vertical-align: center;
  }*/

  .page {
    background-color: #000;
  }

  .header{
    background-color: rgb(69, 69, 69);
    color: white
  }

  .grid {
    margin: 20px;
  }

  .result {
    width: auto;
    text-align: right;
    font-size: 46px;
    margin-left: 40px;
    margin-right: 40px;
    color: white;
  }

  .btnC {
    background-color: rgb(152, 152, 152);
    color:white;
    border: solid;
    border-width: 2px;
    border-color: #000;
    width: 1000px;
    height: 250px;
    margin: 5px;
    border-radius: 200px;
    font-size: 40px;
  }

  .btnC:active {
    background-color: #ffb2b2;
  }

  .btn0 {
    background-color: rgb(86, 86, 86);
    color:white;
    border: solid;
    border-width: 2px;
    border-color: #000;
    width: 1000px;
    height: 250px;
    margin: 5px;
    border-radius: 200px;
    font-size: 40px;
  }

  .btn0:active {
    background-color: #a99f8e;
  }

  .btn{
    background-color: rgb(86, 86, 86);
    color:white;
    border: solid;
    border-width: 2px;
    border-color: #000;
    width: 250px;
    height: 250px;
    margin: 5px;
    border-radius: 200px;
    font-size: 40px;
  }

  .btn:active {
    background-color: #a99f8e;
  }

  .btnAction{
    background-color: rgb(232, 163, 79);
    color:white;
    border: solid;
    border-width: 2px;
    border-color: #000;
    width: 250px;
    height: 250px;
    margin: 5px;
    border-radius: 200px;
    font-size: 40px;
  }

  .btnAction:active {
    background-color: #ffcb83;
  }
</style>
