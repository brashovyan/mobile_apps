<template>
    <Page>
        <ActionBar class="bar">
            <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="home"/>
            <Label text="Викторина" class="bar__text"/>
        </ActionBar>

        <ScrollView>
            <FlexboxLayout class="main">
                <FlexBoxLayout class="scores">
                    <FlexboxLayout>
                        <label text="Ваши очки: " class="record"/>
                        <label :text="score" class="record" />
                    </FlexboxLayout>

                    <FlexboxLayout>
                        <label text="Ваш рекорд: " class="record"/>
                        <label :text="record" class="record" />
                    </FlexboxLayout>
                </FlexBoxLayout>
                
                <FlexBoxLayout class="time__life">
                    <FlexboxLayout>
                        <label text="Время: " class="time"/>
                        <label :text="time" class="time" />
                    </FlexboxLayout>

                    <FlexboxLayout>
                        <label text="Жизней: " class="lifes"/>
                        <label :text="life" class="lifes" />
                    </FlexboxLayout>
                </FlexBoxLayout>
            
                <FlexboxLayout class="question__number__div">
                    <label :text="question_number" class="question__number"/>
                </FlexboxLayout>

                <FlexboxLayout class="question__div">
                    <TextView v-model="question" editable="false" class="question"/>
                </FlexboxLayout>

                <FlexboxLayout class="input__div">
                    <TextField v-model="input" hint="Ответ" class="input"/>
                </FlexboxLayout>

                <FlexboxLayout class="answer__div">
                    <button text="Ответить" @tap="answer_func" class="answer__btn"/>
                </FlexboxLayout>

                <FlexboxLayout>
                    <TextView v-model="answer" editable="false" class="textview"/>
                </FlexboxLayout>
            </FlexboxLayout>
        </ScrollView>
    </Page>
</template>

<script>
  import * as ApplicationSettings from '@nativescript/core/application-settings';
  import { Http } from '@nativescript/core';
  import { Dialogs } from '@nativescript/core';
  import Home from "./Home.vue";
  export default {

    data() {
        return {
            record: 0,
            record__json: [],
            score: 0,
            question_number: "",
            number: 1,
            question: "Пожалйуста подождите!",
            input: "",
            answer: "",
            life: 3,

            sec: 0,
            min: 0,
            hrs: 0,
            time: "",
        };
    },

    mounted() {
        if (ApplicationSettings.getString("record__json")) { // получаем из памяти всё, что хранится в переменной record__json
            this.record__json = Object.values(JSON.parse(ApplicationSettings.getString("record__json"))); // там хранится json. Эти методы грубо говоря парсят его в обычный список
        }
        try {
            this.record = this.record__json[0]["record"];
        }
        catch {
            this.record = 0;
        }
        this.timer();
        this.get_question();
    },

    methods: {

        answer_func() {
            if(this.input != "" && this.answer != ""){

                if(this.input.toLowerCase() == this.answer.toLowerCase()){
                    this.score += 1;
                    this.new_record();
                }
                else{

                    this.life -= 1;
                    if(this.life <= 0){
                            const alertOptions = {
                            title: 'Вы проиграли!',
                            message: `Набрано очков: ${this.score}`,
                            okButtonText: 'Ok',
                            cancelable: false // [Android only] Gets or sets if the dialog can be canceled by taping outside of the dialog.
                        }

                        Dialogs.alert(alertOptions).then(() => {
                            this.$navigateTo(Home);
                        })
                    }
                }

                this.number += 1;
                this.get_question();
            }
        },

        home() {
            this.$navigateBack();
        },

        get_question() {
            this.question = "Пожалйуста подождите!";
            this.answer = "";
            this.input = "";
            this.question_number = `Вопрос № ${this.number}`;
            Http.getJSON("https://jservice.io/api/random").then((result) => {
                this.question = result[0]["question"];
                this.answer = result[0]["answer"];
            });
        },

        new_record() {
            if (Number(this.score) > Number(this.record)) {
                this.record__json = [];
                this.record__json.push({
                    record: Number(this.score),
                });
                this.record = Number(this.score);
                let toSave = Object.assign({}, this.record__json);
                ApplicationSettings.setString("record__json", JSON.stringify(toSave));
            }
        },



        timer() {
            let t = setTimeout(this.add, 1000);
        },

        add() {
            this.tick();
            this.time = (this.hrs > 9 ? this.hrs : "0" + this.hrs) 
                    + ":" + (this.min > 9 ? this.min : "0" + this.min)
                    + ":" + (this.sec > 9 ? this.sec : "0" + this.sec);
            this.timer();
        },

        tick(){
            this.sec++;
            if (this.sec >= 60) {
                this.sec = 0;
                this.min++;
                if (this.min >= 60) {
                    this.min = 0;
                    this.hrs++;
                }
            }
        }
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
        flex-direction: column;
    }

    .scores{
        display: flex;
        align-items: flex-end;
        justify-content: space-between;
        flex-direction: row;
        margin: 30px;
    }

    .record{
        font-size: 18px;
        color: rgb(255, 255, 255);
    }

    .time__life{
        display: flex;
        align-items: flex-end;
        justify-content: space-between;
        flex-direction: row;
        margin: 30px;
    }

    .time{
        font-size: 18px;
        color: rgb(255, 255, 255);
    }

    .lifes{
        font-size: 18px;
        color: rgb(255, 255, 255);
    }

    .question__number__div{
        display: flex;
        align-items: center;
        justify-content: center;
        margin-top: 50px;
    }

    .question__number{
        font-size: 26px;
        color: rgb(255, 255, 255);
    }

    .question__div{
        height: auto;
    }

    .question{
        font-size: 18px;
        color: rgb(255, 255, 255);
    }

    .input{
        width: 100%;
        background-color: white;
        color: black;
        font-size: 20px;
        border-radius: 10px;
    }

    .answer__div{
        display: flex;
        align-items: center;
        justify-content: center;
        margin-top: 20px;
    }
    
    .answer__btn{
        border-radius: 50px;
        background-color: rgb(0, 89, 145);
        font-size: 20px;
        color: white;
    }
</style>
