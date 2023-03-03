<template>
  <Page>
    <ActionBar>
      <Label text="Конвертор" class="action__label"/>
    </ActionBar>
    
    <FlexboxLayout class="flexbox">
      <SearchBar v-model="searchPhrase" hint="Поиск" class="search"/>

      <ScrollView>
        <FlexboxLayout class="flexbox__categories" >
          <FlexboxLayout v-for="(category, key) in filteredCategories" :key="key">
            <button  :text="category.text" @tap="category.tapFunction" class="category" :backgroundImage="category.image"/>
          </FlexboxLayout>
        </FlexboxLayout>
      </ScrollView>
      
    </FlexboxLayout>
  </Page>
</template>

<script lang="js">
import Vue from "nativescript-vue";
import DistanceVue from "./Distance.vue";
import SquareVue from "./Square.vue";
import TemperatureVue from "./Temperature.vue";
import VolumeVue from "./Volume.vue";
import WeightVue from "./Weight.vue";
import CurrencyVue from "./Currency.vue";

  export default Vue.extend({
    data() {
        return {
          searchPhrase: "",

          categories: [],
        };
      },

    mounted() {
      this.categories = [{text: "Длина и расстоя- ние (m)", tapFunction: this.distance, image: "~/img/distance.png"},
          {text: 'Площадь (㎡)', tapFunction: this.square, image: "~/img/square.png"},
          {text: 'Объём (㎥)', tapFunction: this.volume, image: "~/img/volume.jpg"},
          {text: 'Вес (масса, kg)', tapFunction: this.weight, image: "~/img/weight.png"},
          {text: 'Температу-ра (℃)', tapFunction: this.temperature, image: "~/img/temperature.jpg"},
          {text: 'Валюта ($)', tapFunction: this.currency, image: "~/img/currency.png"}]
    },

    computed: {
      filteredCategories(){
        return this.categories.filter((item) => item.text.toLowerCase().includes(this.searchPhrase.toLowerCase()));
      },
    },

    methods: {
      distance(){
        this.$navigateTo(DistanceVue)
      },

      square(){
        this.$navigateTo(SquareVue)
      },

      volume(){
        this.$navigateTo(VolumeVue)
      },

      weight() {
        this.$navigateTo(WeightVue)
      },

      temperature() {
        this.$navigateTo(TemperatureVue)
      },
      
      currency() {
        this.$navigateTo(CurrencyVue)
      }
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

  .search {
    background-color: rgb(255, 255, 255);
    width: 93%;
    height: 100px;
    color: black;
    font-size: 20px;
    margin-top: 30px;
    margin-left: 35px;
  }

  .flexbox{
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: flex-start;
    height: auto;
    width: auto;
  }

  .flexbox__categories{
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    margin-top: 30px;
    height: auto;
    width: auto;
  }

  .category{
    font-size: 16px;
    width: 287px;
    height: 450px;
    background-color: rgb(255, 255, 255);
    color: black;
    border-radius: 10px;
    background-position: 50% 5%;
    background-size: 200px 200px;
    background-repeat: no-repeat;
    padding-top: 150px;
  }

  .category:active
  {
    background-color: rgb(234, 255, 235);
    color: black;
  }
</style>
