<template>
    <Page>
        <ActionBar class="bar">
            <Label text="Викторина" class="bar__text"/>
        </ActionBar>

        <ScrollView>
            <FlexboxLayout class="main">
                <FlexboxLayout>
                    <label text="Добро пожаловать на викторину!" class="welcome"/>
                </FlexboxLayout>

                <FlexboxLayout>
                    <label text="Ваш рекорд: " class="record"/>
                    <label :text="record" class="record" />
                </FlexboxLayout>

                <FlexboxLayout>
                    <button  text="Начать" @tap="start" class="start"/>
                </FlexboxLayout>

            </FlexboxLayout>
        </ScrollView>
    </Page>
</template>

<script>
  import * as ApplicationSettings from '@nativescript/core/application-settings';
  import Quiz from "./Quiz.vue";
  export default {
    data() {
        return {
           record: 0,
           record__json: [],
        };
    },

    mounted() {
        if (ApplicationSettings.getString("record__json")) { // получаем из памяти всё, что хранится в переменной record__json
            this.record__json = Object.values(JSON.parse(ApplicationSettings.getString("record__json"))); // там хранится json. Эти методы грубо говоря парсят его в обычный список
        }

        try{
            this.record = this.record__json[0]['record'];
        }
        catch{
            this.record = 0;
        }  
    },

    methods: {
        start() {
            this.$navigateTo(Quiz);
        },
    },
};
</script>

<style scoped lang="scss">
    @import '@nativescript/theme/scss/variables/blue';

    page{
        background-image: url("~/img/background.jpg");
        background-size: 100% 100%;
        background-repeat: no-repeat;
        background-color: white;
    }

    .bar{
        background-color: rgb(0, 42, 73);
    }

    .bar__text{
        color: rgb(255, 255, 255);
    }

    .main{
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
    }

    .welcome{
        font-size: 20px;
        color: rgb(255, 255, 255);
        text-align: center;
    }

    .record{
        font-size: 18px;
        color: rgb(255, 255, 255);
    }

    .start{
        border-radius: 50px;
        background-color: rgb(0, 89, 145);
        font-size: 20px;
        color: white;
    }
</style>
