<template>
    <Page class="page">
        <ActionBar>
            <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home"/>
            <Label text="Задача" class="header"/>
        </ActionBar>
  
        <FlexboxLayout class="flexbox">
            <TextField v-model="title" />
            <TextField v-model="description" />
            <button @tap="change()" text="Изменить" />
            <button @tap="del()" text="Удалить" />
            <button @tap="done()" text="Выполнено" />
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
                // СДЕЛАТЬ ЗАЩИТУ ОТ ДУРАКА!!!
                if(task.id == this.id){
                    if(this.title != task.title && (this.title != "" || this.title.length != 0)){
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

    .header {
        color: white;
        font-size: 26px;
    }
    
    .flexbox {
        display: flex;
        flex-direction: column;
    }
  </style>
  