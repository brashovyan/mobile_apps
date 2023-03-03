<template>
  <Page>
    <ActionBar>
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home" />
      <Label text="Площадь" class="action__label"/>
    </ActionBar>

    <FlexboxLayout class="flexbox__main">
      <FlexboxLayout class="flexbox__input">
        <TextField v-model="textFieldValue" keyboardType="number" maxLength="15" hint="Введите величину" class="textfield"/>
        <ListPicker :items="measure" v-model="selectedMeasure" class="selector"/>
      </FlexboxLayout>

      <ScrollView>
        <FlexboxLayout class="flexbox__output">
        <label :text="mm" class="label"/>
        <label :text="cm" class="label"/>
        <label :text="m" class="label"/>
        <label :text="km" class="label"/>
        <label :text="hectar" class="label"/>
        <label :text="sotka" class="label"/>
      </FlexboxLayout>
      </ScrollView> 
    </FlexboxLayout>
    
  </Page>
</template>

<script lang="js">
import Vue from "nativescript-vue";
import HomeVue from "./Home.vue";

export default Vue.extend({
  data() {
      return {
        textFieldValue: "0",
        measure: [],
        selectedMeasure: "",

        mm: "",
        cm: "",
        m: "",
        km: "",
        hectar: "",
        sotka: "",
      };
    },

  mounted() {
    this.measure = ['кв.мм', 'кв.см', 'кв.м', "кв.км", "гектары", "сотки"]
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
      if(this.selectedMeasure == 0 || isNaN(this.selectedMeasure)){ // кв.мм
        this.mm = `кв.мм: ${this.textFieldValue/1}`;
        this.cm = `кв.см: ${this.textFieldValue*0.01}`
        this.m = `кв.м: ${this.textFieldValue*0.000001}`
        this.km = `кв.км: ${this.textFieldValue*0.000000000001}`
        this.hectar = `гектары: ${this.textFieldValue*0.0000000001}`
        this.sotka = `сотки: ${this.textFieldValue*0.00000001}` 
      }
      else if(this.selectedMeasure == 1){ // кв.см
        this.mm = `кв.мм: ${this.textFieldValue*100}`;
        this.cm = `кв.см: ${this.textFieldValue*1}`
        this.m = `кв.м: ${this.textFieldValue*0.0001}`
        this.km = `кв.км: ${this.textFieldValue*0.0000000001}`
        this.hectar = `гектары: ${this.textFieldValue*0.00000001}`
        this.sotka = `сотки: ${this.textFieldValue*0.000001}` 
      }
      else if(this.selectedMeasure == 2){ // кв.м
        this.mm = `кв.мм: ${this.textFieldValue*1000000}`;
        this.cm = `кв.см: ${this.textFieldValue*10000}`
        this.m = `кв.м: ${this.textFieldValue/1}`
        this.km = `кв.км: ${this.textFieldValue*0.000001}`
        this.hectar = `гектары: ${this.textFieldValue*0.0001}`
        this.sotka = `сотки: ${this.textFieldValue*0.01}` 
      }
      else if(this.selectedMeasure == 3){ // кв.км
        this.mm = `кв.мм: ${this.textFieldValue*1000000000000}`;
        this.cm = `кв.см: ${this.textFieldValue*10000000000}`
        this.m = `кв.м: ${this.textFieldValue*1000000}`
        this.km = `кв.км: ${this.textFieldValue*1}`
        this.hectar = `гектары: ${this.textFieldValue*100}`
        this.sotka = `сотки: ${this.textFieldValue*10000}` 
      }
      else if(this.selectedMeasure == 4){ // гектары
        this.mm = `кв.мм: ${this.textFieldValue*10000000000}`;
        this.cm = `кв.см: ${this.textFieldValue*100000000}`
        this.m = `кв.м: ${this.textFieldValue*10000}`
        this.km = `кв.км: ${this.textFieldValue*0.01}`
        this.hectar = `гектары: ${this.textFieldValue*1}`
        this.sotka = `сотки: ${this.textFieldValue*100}` 
      }
      else if(this.selectedMeasure == 5){ // сотки
        this.mm = `кв.мм: ${this.textFieldValue*100000000}`;
        this.cm = `кв.см: ${this.textFieldValue*1000000}`
        this.m = `кв.м: ${this.textFieldValue*100}`
        this.km = `кв.км: ${this.textFieldValue*0.0001}`
        this.hectar = `гектары: ${this.textFieldValue*0.01}`
        this.sotka = `сотки: ${this.textFieldValue*1}` 
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
</style>