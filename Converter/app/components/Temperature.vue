<template>
  <Page>
    <ActionBar>
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home" />
      <Label text="Температура" class="action__label"/>
    </ActionBar>

    <FlexboxLayout class="flexbox__main">
      <FlexboxLayout class="flexbox__input">
        <TextField v-model="textFieldValue" keyboardType="number" maxLength="15" hint="Введите величину" class="textfield" :text="textFieldValue"/>
        <ListPicker :items="measure" v-model="selectedMeasure" class="selector"/>
      </FlexboxLayout>

      <ScrollView>
        <FlexboxLayout class="flexbox__output">
        <label :text="c" class="label"/>
        <label :text="k" class="label"/>
        <label :text="f" class="label"/>
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

        c: "",
        k: "",
        f: "",
      };
    },

  mounted() {
    this.measure = ['℃', 'K', '℉']
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
      if(this.textFieldValue != ""){ //если поле значения не пустое
        if(this.selectedMeasure == 0 || isNaN(this.selectedMeasure)){ // с
          this.c = `℃: ${parseFloat(this.textFieldValue)*1}`;
          this.k = `K: ${parseFloat(this.textFieldValue)+273.15}`
          this.f = `℉': ${eval(parseFloat(this.textFieldValue)*1.8+32)}`
        }
        else if(this.selectedMeasure == 1){ // k
          this.c = `℃: ${parseFloat(this.textFieldValue)-273.15}`;
          this.k = `K: ${parseFloat(this.textFieldValue)*1}`
          this.f = `℉': ${eval((parseFloat(this.textFieldValue)-273.15)*1.8 + 32)}`
        }
        else if(this.selectedMeasure == 2){ // f
          this.c = `℃: ${eval((parseFloat(this.textFieldValue)-32)*0.555555555)}`;
          this.k = `K: ${eval(((parseFloat(this.textFieldValue)-32)*0.555555555)+273.15)}`
          this.f = `℉': ${parseFloat(this.textFieldValue)*1}`
        }
      }
      else{
        if(this.selectedMeasure == 0 || isNaN(this.selectedMeasure)){ // с
          this.c = `℃: 0`;
          this.k = `K: 273.15`
          this.f = `℉': 32`
        }
        else if(this.selectedMeasure == 1){ // k
          this.c = `℃: -273.15`;
          this.k = `K: 0`
          this.f = `℉': -459.67`
        }
        else if(this.selectedMeasure == 2){ // f
          this.c = `℃: -17.78`;
          this.k = `K: 255.37`
          this.f = `℉': 0`
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
</style>