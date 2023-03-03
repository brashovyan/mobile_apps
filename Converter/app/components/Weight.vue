<template>
  <Page>
    <ActionBar>
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home" />
      <Label text="Вес" class="action__label"/>
    </ActionBar>

    <FlexboxLayout class="flexbox__main">
      <FlexboxLayout class="flexbox__input">
        <TextField v-model="textFieldValue" keyboardType="number" maxLength="15" hint="Введите величину" class="textfield"/>
        <ListPicker :items="measure" v-model="selectedMeasure" class="selector"/>
      </FlexboxLayout>

      <ScrollView>
        <FlexboxLayout class="flexbox__output">
        <label :text="g" class="label"/>
        <label :text="kg" class="label"/>
        <label :text="centner" class="label"/>
        <label :text="t" class="label"/>
        <label :text="ounce" class="label"/>
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

        g: "",
        kg: "",
        centner: "",
        t: "",
        ounce: "",
      };
    },

  mounted() {
    this.measure = ['г', 'кг', 'ц', "т", "унций"]
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
      if(this.selectedMeasure == 0 || isNaN(this.selectedMeasure)){ // г
        this.g = `грамм: ${this.textFieldValue*1}`;
        this.kg = `килограмм: ${this.textFieldValue*0.001}`
        this.centner = `центнеров: ${this.textFieldValue*0.00001}`
        this.t = `тонн: ${this.textFieldValue*0.000001}`
        this.ounce = `унций: ${this.textFieldValue*0.03527396195}`
      }
      else if(this.selectedMeasure == 1){ // кг
        this.g = `грамм: ${this.textFieldValue*1000}`;
        this.kg = `килограмм: ${this.textFieldValue*1}`
        this.centner = `центнеров: ${this.textFieldValue*0.01}`
        this.t = `тонн: ${this.textFieldValue*0.001}`
        this.ounce = `унций: ${this.textFieldValue*35.27396194958}`
      }
      else if(this.selectedMeasure == 2){ // ц
        this.g = `грамм: ${this.textFieldValue*100000}`;
        this.kg = `килограмм: ${this.textFieldValue*100}`
        this.centner = `центнеров: ${this.textFieldValue*1}`
        this.t = `тонн: ${this.textFieldValue*0.1}`
        this.ounce = `унций: ${this.textFieldValue*3527.396194958}`
      }
      else if(this.selectedMeasure == 3){ // т
        this.g = `грамм: ${this.textFieldValue*1000000}`;
        this.kg = `килограмм: ${this.textFieldValue*1000}`
        this.centner = `центнеров: ${this.textFieldValue*10}`
        this.t = `тонн: ${this.textFieldValue*1}`
        this.ounce = `унций: ${this.textFieldValue*35273.96194958}`
      }
      else if(this.selectedMeasure == 4){ // унций
        this.g = `грамм: ${this.textFieldValue*28.349523125}`;
        this.kg = `килограмм: ${this.textFieldValue*0.02834952313}`
        this.centner = `центнеров: ${this.textFieldValue*0.00028349523}`
        this.t = `тонн: ${this.textFieldValue*0.000028349523125}`
        this.ounce = `унций: ${this.textFieldValue*1}`
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