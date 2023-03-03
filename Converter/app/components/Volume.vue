<template>
  <Page>
    <ActionBar>
      <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home" />
      <Label text="Объём" class="action__label"/>
    </ActionBar>

    <FlexboxLayout class="flexbox__main">
      <FlexboxLayout class="flexbox__input">
        <TextField v-model="textFieldValue" keyboardType="number" maxLength="15" hint="Введите величину" class="textfield"/>
        <ListPicker :items="measure" v-model="selectedMeasure" class="selector"/>
      </FlexboxLayout>

      <ScrollView>
        <FlexboxLayout class="flexbox__output">
        <label :text="cm" class="label"/>
        <label :text="m" class="label"/>
        <label :text="km" class="label"/>
        <label :text="mliters" class="label"/>
        <label :text="liters" class="label"/>
        <label :text="barrels" class="label"/>
        <label :text="gallons" class="label"/>
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

        cm: "",
        m: "",
        km: "",
        mliters: "",
        liters: "",
        barrels: "",
        gallons: "",
      };
    },

  mounted() {
    this.measure = ['куб.см', 'куб.м', "куб.км", "миллилитр", "литры", "баррели", "галлоны"]
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
      if(this.selectedMeasure == 0 || isNaN(this.selectedMeasure)){ // куб.см
        this.cm = `куб.см: ${this.textFieldValue*1}`
        this.m = `куб.м: ${this.textFieldValue*0.000001}`
        this.km = `куб.км: ${this.textFieldValue*0.000000000000001}`
        this.mliters = `миллилитры: ${this.textFieldValue*1}`
        this.liters = `литры: ${this.textFieldValue*0.001}`
        this.barrels = `баррели (США): ${this.textFieldValue*0.0000062898108}`
        this.gallons = `галлоны (США): ${this.textFieldValue*0.00026417205}`
      }
      else if(this.selectedMeasure == 1){ // куб.м
        this.cm = `куб.см: ${this.textFieldValue*1000000}`
        this.m = `куб.м: ${this.textFieldValue*1}`
        this.km = `куб.км: ${this.textFieldValue*0.000000001}`
        this.mliters = `миллилитры: ${this.textFieldValue*1000000}`
        this.liters = `литры: ${this.textFieldValue*1000}`
        this.barrels = `баррели (США): ${this.textFieldValue*6.2898108}`
        this.gallons = `галлоны (США): ${this.textFieldValue*264.17205}`
      }
      else if(this.selectedMeasure == 2){ // куб.км
        this.cm = `куб.см: ${this.textFieldValue*1000000000000000}`
        this.m = `куб.м: ${this.textFieldValue*1000000000}`
        this.km = `куб.км: ${this.textFieldValue*1}`
        this.mliters = `миллилитры: ${this.textFieldValue*1000000000000000}`
        this.liters = `литры: ${this.textFieldValue*1000000000000}`
        this.barrels = `баррели (США): ${this.textFieldValue*6289810800}`
        this.gallons = `галлоны (США): ${this.textFieldValue*264172050000}`
      }
      else if(this.selectedMeasure == 3){ // миллилитр
        this.cm = `куб.см: ${this.textFieldValue*1}`
        this.m = `куб.м: ${this.textFieldValue*0.000001}`
        this.km = `куб.км: ${this.textFieldValue*0.000000000000001}`
        this.mliters = `миллилитры: ${this.textFieldValue*1}`
        this.liters = `литры: ${this.textFieldValue*0.001}`
        this.barrels = `баррели (США): ${this.textFieldValue*0.0000062898108}`
        this.gallons = `галлоны (США): ${this.textFieldValue*0.00026417205}`
      }
      else if(this.selectedMeasure == 4){ // литры
        this.cm = `куб.см: ${this.textFieldValue*1000}`
        this.m = `куб.м: ${this.textFieldValue*0.001}`
        this.km = `куб.км: ${this.textFieldValue*0.000000000001}`
        this.mliters = `миллилитры: ${this.textFieldValue*1000}`
        this.liters = `литры: ${this.textFieldValue*1}`
        this.barrels = `баррели (США): ${this.textFieldValue*0.0062898108}`
        this.gallons = `галлоны (США): ${this.textFieldValue*0.26417205}`
      }
      else if(this.selectedMeasure == 5){ // баррели
        this.cm = `куб.см: ${this.textFieldValue*158987.29418061}`
        this.m = `куб.м: ${this.textFieldValue*0.15898729418}`
        this.km = `куб.км: ${this.textFieldValue*0.00000000015898729418}`
        this.mliters = `миллилитры: ${this.textFieldValue*158987.29418061}`
        this.liters = `литры: ${this.textFieldValue*158.98729418061}`
        this.barrels = `баррели (США): ${this.textFieldValue*1}`
        this.gallons = `галлоны (США): ${this.textFieldValue*41.99999942765}`
      }
      else if(this.selectedMeasure == 6){ // галлоны
        this.cm = `куб.см: ${this.textFieldValue*3785.4118177907}`
        this.m = `куб.м: ${this.textFieldValue*0.00378541182}`
        this.km = `куб.км: ${this.textFieldValue*0.00000000000378541181}`
        this.mliters = `миллилитры: ${this.textFieldValue*3785.4118177907}`
        this.liters = `литры: ${this.textFieldValue*3.78541181779}`
        this.barrels = `баррели (США): ${this.textFieldValue*0.02380952413}`
        this.gallons = `галлоны (США): ${this.textFieldValue*1}`
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