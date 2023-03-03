<template>
    <Page>
      <ActionBar>
        <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home" />
        <Label text="Расстояние" class="action__label"/>
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
          <label :text="miles" class="label"/>
          <label :text="feet" class="label"/>
          <label :text="inches" class="label"/>
          <label :text="yards" class="label"/>
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
          miles: "",
          feet: "",
          inches: "",
          yards: "",
        };
      },

    mounted() {
      this.measure = ['мм', 'см', 'м', "км", "мили", "футы", "дюймы", "ярды"]
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
        if(this.selectedMeasure == 0 || isNaN(this.selectedMeasure)){ // мм
          this.mm = `мм: ${this.textFieldValue/1}`;
          this.cm = `см: ${this.textFieldValue/10}`
          this.m = `м: ${this.textFieldValue/1000}`
          this.km = `км: ${this.textFieldValue/1000000}`
          this.miles = `мили: ${this.textFieldValue/1610305.96}`
          this.feet = `футы: ${this.textFieldValue/303.03}`
          this.inches = `дюймы: ${this.textFieldValue/25.641}`
          this.yards = `ярды: ${this.textFieldValue/909.09}`
          
        }
        else if(this.selectedMeasure == 1){ // см
          this.mm = `мм: ${this.textFieldValue*10}`;
          this.cm = `см: ${this.textFieldValue/1}`
          this.m = `м: ${this.textFieldValue/100}`
          this.km = `км: ${this.textFieldValue/100000}`
          this.miles = `мили: ${this.textFieldValue/161290.3}`
          this.feet = `футы: ${this.textFieldValue/30.3}`
          this.inches = `дюймы: ${this.textFieldValue/2.5641}`
          this.yards = `ярды: ${this.textFieldValue/90.9}`
          
        }
        else if(this.selectedMeasure == 2){ // м
          this.mm = `мм: ${this.textFieldValue*1000}`;
          this.cm = `см: ${this.textFieldValue*100}`
          this.m = `м: ${this.textFieldValue/1}`
          this.km = `км: ${this.textFieldValue/1000}`
          this.miles = `мили: ${this.textFieldValue/1612.9}`
          this.feet = `футы: ${this.textFieldValue*3.28}`
          this.inches = `дюймы: ${this.textFieldValue*39.37}`
          this.yards = `ярды: ${this.textFieldValue*1.09}`
          
        }
        else if(this.selectedMeasure == 3){ // км
          this.mm = `мм: ${this.textFieldValue*1000000}`;
          this.cm = `см: ${this.textFieldValue*100000}`
          this.m = `м: ${this.textFieldValue*1000}`
          this.km = `км: ${this.textFieldValue/1}`
          this.miles = `мили: ${this.textFieldValue/1.6129}`
          this.feet = `футы: ${this.textFieldValue*3280.84}`
          this.inches = `дюймы: ${this.textFieldValue*39370.08}`
          this.yards = `ярды: ${this.textFieldValue*1093.61}`
          
        }
        else if(this.selectedMeasure == 4){ // мили
          this.mm = `мм: ${this.textFieldValue*1609344}`;
          this.cm = `см: ${this.textFieldValue*160934.4}`
          this.m = `м: ${this.textFieldValue*1609.34}`
          this.km = `км: ${this.textFieldValue*1.61}`
          this.miles = `мили: ${this.textFieldValue/1}`
          this.feet = `футы: ${this.textFieldValue*5280}`
          this.inches = `дюймы: ${this.textFieldValue*63360}`
          this.yards = `ярды: ${this.textFieldValue*1760}`
          
        }
        else if(this.selectedMeasure == 5){ // футы
          this.mm = `мм: ${this.textFieldValue*304.8}`;
          this.cm = `см: ${this.textFieldValue*30.48}`
          this.m = `м: ${this.textFieldValue*0.3}`
          this.km = `км: ${this.textFieldValue*0.0003}`
          this.miles = `мили: ${this.textFieldValue*0.00019}`
          this.feet = `футы: ${this.textFieldValue*1}`
          this.inches = `дюймы: ${this.textFieldValue*12}`
          this.yards = `ярды: ${this.textFieldValue*0.33}`
          
        }
        else if(this.selectedMeasure == 6){ // дюймы
          this.mm = `мм: ${this.textFieldValue*25.4}`;
          this.cm = `см: ${this.textFieldValue*2.54}`
          this.m = `м: ${this.textFieldValue*0.025}`
          this.km = `км: ${this.textFieldValue*0.000025}`
          this.miles = `мили: ${this.textFieldValue*0.000016}`
          this.feet = `футы: ${this.textFieldValue*0.083}`
          this.inches = `дюймы: ${this.textFieldValue*1}`
          this.yards = `ярды: ${this.textFieldValue*0.028}`
          
        }  
        else if(this.selectedMeasure == 7){ // ярды
          this.mm = `мм: ${this.textFieldValue*914.4}`;
          this.cm = `см: ${this.textFieldValue*91.44}`
          this.m = `м: ${this.textFieldValue*0.91}`
          this.km = `км: ${this.textFieldValue*0.00091}`
          this.miles = `мили: ${this.textFieldValue*0.00057}`
          this.feet = `футы: ${this.textFieldValue*3}`
          this.inches = `дюймы: ${this.textFieldValue*36}`
          this.yards = `ярды: ${this.textFieldValue*1}`
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