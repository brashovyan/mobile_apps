<template>
    <Page>
        <ActionBar>
            <Label text="Список задач" class="header"/>
        </ActionBar>

        <FlexboxLayout class="flexbox">

            <!-- выбор фильтра -->
            <FlexboxLayout>
                <SegmentedBar v-model="selectedFilter" class="segment" selectedBackgroundColor="white">
                    <SegmentedBarItem title="Все"/>
                    <SegmentedBarItem title="Выполнено"/>
                    <SegmentedBarItem title="В процессе"/>
                </SegmentedBar>
            </FlexboxLayout>

            <!-- поиск -->
            <FlexboxLayout class="flexbox__search">
                <SearchBar v-model="searchPhrase" hint="Поиск" class="search"/>
            </FlexboxLayout>
            
            <!-- здачи -->
            <FlexboxLayout class="flexbox__main">
                <scrollView>
                    <FlexboxLayout class="flexbox__tasks" >
                        <template v-for="(task, key) in filteredTasks" >
                            <template v-if="task.done">
                                <FlexboxLayout class="task__done" :key="key" @tap="click(task.id)">
                                    <label class="title" :text="task.title" />
                                    <label class="description" :text="task.description" />
                                    <FlexboxLayout class="date">
                                        <label class="date__text" :text="task.date" />
                                    </FlexboxLayout>
                                </FlexboxLayout>
                            </template>
                            <template v-else>
                                <FlexboxLayout class="task__no" :key="key" @tap="click(task.id)">
                                    <label class="title" :text="task.title" />
                                    <label class="description" :text="task.description" />
                                    <FlexboxLayout class="date">
                                        <label class="date__text" :text="task.date" />
                                    </FlexboxLayout>
                                </FlexboxLayout>
                            </template>
                        </template>
                    </FlexboxLayout>
                </scrollView>
            </FlexboxLayout>

            <!-- добавить задачу -->
            <FlexboxLayout class="add__task">
                <button text="+" class="button__task" @tap="add()" />
            </FlexboxLayout>
        </FlexboxLayout>
    </Page>
</template>

<script>
import * as ApplicationSettings from '@nativescript/core/application-settings';
import { LocalNotifications } from '@nativescript/local-notifications';
import Add from "./Add.vue";
import Detail from './Detail.vue';
  export default {
    data() {
        return {
            searchPhrase: "", // строка поиска
            selectedFilter: "", // выбранный фильтр
            tasks: [], // общий список задач
            filteredTasks: [], // отфильтрованный список задач
        };
    },

    created() {
        LocalNotifications.hasPermission();
        LocalNotifications.schedule([
        {
            id: 1, 
            title: 'ToDo',
            body: 'Не забудь выполнить свои задачи!',
            ongoing: false, // сможет ли юзер смахнуть уведомление (false - сможет)
            thumbnail: false,
            forceShowWhenInForeground: true,
            interval: 'day', // c каким интервалом (каждую минуту)
            channel: 'My Channel', // default: 'Channel'
            at: new Date(new Date().getTime()) 
        }
        ]);
    },

    mounted() {
        if (ApplicationSettings.getString("tasks")) { // получаем из памяти всё, что хранится в переменной tasks
            this.tasks = Object.values(JSON.parse(ApplicationSettings.getString("tasks"))); // там хранится json. Эти методы грубо говоря парсят его в обычный список
            // получается tasks - это список словарей [{1: "1", 2: "2"}, {"yes": "no"}]
        }
        this.tasks = this.tasks.reverse();
        this.filter();
    },

    watch: { 
      selectedFilter: function(){ // при изменении фильтра
        this.filter();
      },

      searchPhrase: function(){ // при поиске
        this.search();
      },
    },

    methods: {
        click(id) { // тыкаем на задачу
            this.$navigateTo(Detail, { props: {id: id}});
        },

        add() { // добавить задачу
            this.$navigateTo(Add);
        },

        filter(){ // фильтр
            
            if(this.selectedFilter == 0 || isNaN(this.selectedFilter)){ // все
                this.filteredTasks = [];
                this.filteredTasks = this.tasks;
                
            }

            else if (this.selectedFilter == 1){ // выполнено
                this.filteredTasks = [];
                this.tasks.forEach(task => {
                    if(task.done){
                        this.filteredTasks.push({
                            id: task.id,
                            title: task.title,
                            description: task.description,
                            date: task.date,
                            done: task.done,
                        });
                    } 
                });
            }

            else{ // в процессе
                this.filteredTasks = [];
                this.tasks.forEach(task => {
                    if(!task.done){
                        this.filteredTasks.push({
                            id: task.id,
                            title: task.title,
                            description: task.description,
                            date: task.date,
                            done: task.done,
                        });
                    } 
                });
            }
        },

        search() { // поиск
            // ищу по всем задачам
            if(this.searchPhrase != ""){
                this.filteredTasks = [];
                this.tasks.forEach(task => {
                    if(task.title.toLowerCase().includes(this.searchPhrase.toLowerCase()) || task.description.toLowerCase().includes(this.searchPhrase.toLowerCase())){
                        this.filteredTasks.push({
                            id: task.id,
                            title: task.title,
                            description: task.description,
                            date: task.date,
                            done: task.done,
                        });
                    } 
                });
            }
            else{
                this.selectedFilter = "0"; // когда стираем строку поиска, показываем все задачи
            }
            
        },

        
            
    },
};
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

    page {
        background-image: url("~/img/background.jpg");
        background-size: 100% 100%;
        background-repeat: no-repeat;
        background-color: white;
    }

    .flexbox{
        display: flex;
        flex-direction: column;
        height: 100%;
    }

    .flexbox__main {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        height: 75%;
    }

    .flexbox__tasks {
        display: flex;
        flex-direction: column;
    }

    SegmentedBar {
        background-color: rgb(122, 160, 254);
        width: 100%;
    }

    .flexbox__search {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 150px;
    }

    .search {
        background-color: rgb(255, 255, 255);
        width: 90%;
        color: black;
        font-size: 20px;
        margin-top: 20px;
        margin-bottom: 20px;
    }

    .task__done {
        height: 400px;
        width: 90%;
        background-color: rgb(131, 255, 133);
        margin-top: 30px;

        display: flex;
        justify-content: flex-start;
        align-items: flex-start;
        flex-direction: column;
        color: black;
        border-radius: 15px;
    }

    .task__no {
        height: 400px;
        width: 90%;
        background-color: rgb(247, 156, 156);
        margin-top: 30px;

        display: flex;
        justify-content: flex-start;
        align-items: flex-start;
        flex-direction: column;
        color: black;
        border-radius: 15px;
    }

    .title {
        height: 150px;
        padding: 10px;
        font-size: 22px;
    }
    
    .description {
        height: 150px;
        padding: 10px;
        font-size: 18px;
    }

    .date {
        display: flex;
        align-items: flex-end;
        justify-content: flex-end;
        height: 100px;
        width: 100%;
        padding: 10px;
        font-size: 16px;
    }
    
    .add__task {
        display: flex;
        align-items: flex-end;
        justify-content: flex-end;
        margin-top: 10px;
    }

    .button__task {
        border-radius: 100px;
        background-color: rgba(6, 68, 0, 0.925);
        font-size: 30px;
        color: white;
    }
</style>
