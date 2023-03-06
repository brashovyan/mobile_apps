<template>
    <Page class="page">
        <ActionBar>
            <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home"/>
            <Label text="Добавить задачу" class="header"/>
        </ActionBar>
  
        <FlexboxLayout class="flexbox">
            <label text="Введите название задачи:" class="label__title"></label>
            <TextView v-model="title" editable="true" class="title"/>
            <label text="Введите описание задачи:" class="label__description"></label>
            <TextView v-model="description" editable="true" class="description"/>
            <button @tap="add()" text="Сохранить" class="button__save"/>
        </FlexboxLayout>
    </Page>
  </template>
  
  <script>
  import * as ApplicationSettings from '@nativescript/core/application-settings';
  import Vue from "nativescript-vue";
  import Home from './Home.vue';

    export default Vue.extend({
    data() {
        return {
            tasks: [],
            title: "",
            description: "", //описание
        };
    },
    mounted() {
        if (ApplicationSettings.getString("tasks")) { // получаем из памяти всё, что хранится в переменной tasks
            this.tasks = Object.values(JSON.parse(ApplicationSettings.getString("tasks"))); // там хранится json. Эти методы грубо говоря парсят его в обычный список
        }
    },
    methods: {
        home() {
            // кнопка назад без обновления
            this.$navigateBack();
        },
        add() {
            // получается tasks - это список словарей [{1: "1", 2: "2"}, {"yes": "no"}]
            // получение текущего времени
            let current = new Date();
            let day = current.getDate().toString();
            if (day.length == 1) {
                day = "0" + day;
            }
            let month = current.getMonth() + 1;
            month = month.toString();
            if (month.length == 1) {
                month = "0" + month;
            }
            let date = day + "." + month + "." + current.getFullYear();
            let hours = current.getHours().toString();
            if (hours.length == 1) {
                hours = "0" + hours;
            }
            let minutes = current.getMinutes().toString();
            if (minutes.length == 1) {
                minutes = "0" + minutes;
            }
            let time = hours + ":" + minutes;
            let dateTime = date + " " + time;
            //проверка на пустые поля
            if (this.title != "" || this.title.length != 0) {
                if (this.description == "" || this.description.length == 0) { // если описание пустое, я просто делаю его таким же, как название
                    this.description = this.title;
                }
                this.tasks.push({
                    id: Math.random(),
                    title: this.title,
                    description: this.description,
                    date: dateTime,
                    done: false,
                });
                this.save();
            }
            else {
                let options = {
                    title: "Ошибка!",
                    message: "Введите название задачи.",
                    okButtonText: "OK"
                };
                alert(options);
            }
        },
        save() {
            // здесь грубо говоря запаковываем список в json и сохраняем
            let toSave = Object.assign({}, this.tasks);
            ApplicationSettings.setString("tasks", JSON.stringify(toSave));
            this.homeUpdate();
        },
        homeUpdate() {
            this.$navigateTo(Home);
        },
    },
});
  </script>
  
  <style scoped lang="scss">
    @import '@nativescript/theme/scss/variables/blue';

    actionbar {
        background-color: rgb(81, 130, 255);
    }

    page {
        background-image: url("~/img/background.jpg");
        background-size: 100% 100%;
        background-repeat: no-repeat;
        background-color: white;
    }

    .header {
        color: white;
        font-size: 26px;
    }
    
    .flexbox {
        display: flex;
        flex-direction: column;
    }

    .title {
        background-color: white;
        border-radius: 15px;
        font-size: 20px;
        color: black;
    }

    .description {
        background-color: white;
        border-radius: 15px;
        font-size: 18px;
        color: black;
    }

    .label__title {
        margin-left: 6%;
        margin-right: 6%;
        font-size: 24px;
    }

    .label__description {
        margin-left: 6%;
        margin-right: 6%;
        font-size: 24px;
    }

    .button__save {
        border-radius: 100px;
        background-color: rgba(6, 68, 0, 0.925);
        font-size: 30px;
        color: white;
    }

  </style>
  