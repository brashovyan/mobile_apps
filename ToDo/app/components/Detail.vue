<template>
    <Page class="page">
        <ActionBar>
            <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home"/>
            <Label text="Задача" class="header"/>
        </ActionBar>

        <FlexboxLayout class="flexbox">
            <label text="Название задачи:" class="label__title"></label>
            <TextView v-model="title" editable="true" class="title"/>
            <label text="Описание задачи:" class="label__description"></label>
            <TextView v-model="description" editable="true" class="description"/>
            <button @tap="change()" text="Изменить" class="button__save"/>
            <button @tap="del()" text="Удалить" class="button__save"/>
            <button @tap="done()" text="Выполнено" class="button__save"/>
        </FlexboxLayout>
    </Page>
  </template>
  
  <script>
  import * as ApplicationSettings from '@nativescript/core/application-settings';
  import Vue from "nativescript-vue";
  import Home from './Home.vue';

    export default Vue.extend({
    props: ['id'], // айдишник, который мы передали
    data() {
        return {
            tasks: [], // задачи
            task: [], // выбранная задача
            title: "", // название
            description: "", //описание
        };
    },

    mounted() {
        if (ApplicationSettings.getString("tasks")) { 
            this.tasks = Object.values(JSON.parse(ApplicationSettings.getString("tasks"))); 
        }

        this.tasks.forEach(task => { // ищу по айдишнику свою задачу
            if(task.id == this.id){
                this.task = task;
            }
        });

        this.title = this.task.title;
        this.description = this.task.description;
    },

   
    methods: {
        home() {
            this.$navigateBack();
        },

        change(){ // изменить задачу
            this.tasks.forEach(task => { // ищу по айдишнику свою задачу
                if(task.id == this.id){
                    if(this.title != "" || this.title.length != 0){
                        task.title = this.title;
                    }

                    if(this.description == "" || this.description.length == 0 ){
                        this.description == task.description;
                    }
                    task.description = this.description;
                }
            });
            this.save();
        },

        del() {
            let confirmOptions = {
                title: "Удаление задачи",
                message: "Вы точно хотите удалить эту задачу?",
                okButtonText: "Да",
                cancelButtonText: "Нет",
            };

            confirm(confirmOptions).then((result) => {
                if(result == true){
                    let newTasks = [];
                    this.tasks.forEach(task => { // ищу по айдишнику свою задачу
                        if(task.id != this.id){
                            newTasks.push({
                                id: task.id,
                                title: task.title,
                                description: task.description,
                                date: task.date,
                                done: task.done,
                            });
                        }
                    });
                    this.tasks = newTasks;
                    this.save();
                }
            });
        },

        done() {
            this.tasks.forEach(task => { // ищу по айдишнику свою задачу
                if(task.id == this.id){
                    task.done = true;
                }
            });
            this.save();
        },

        save() {
            // здесь грубо говоря запаковываем список в json и сохраняем
            let toSave = Object.assign({}, this.tasks);
            ApplicationSettings.setString("tasks", JSON.stringify(toSave));
            this.homeUpdate();
        },

        homeUpdate(){
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
  