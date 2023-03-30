<template>
    <Page>
        <ActionBar>
            <Label text="Календарь" class="header"/>
        </ActionBar>

        <ScrollView>
            <GridLayout columns="*, *, *, *, *, *, *" rows="auto, auto, auto, auto, auto, auto, auto, auto, auto" clas="grid">
                <FlexboxLayout class="flexbox" col="0" colSpan="7" row="0">
                    <FlexboxLayout class="month" >
                        <FlexboxLayout class="months" col="0" colSpan="5" row="0">
                            <Label :text="month" class="label__month" col="0" colSpan="3" row="0"/>
                            <Label :text="year" class="label__year" col="3" colSpan="2" row="0"/>
                        </FlexboxLayout>
                        

                        <FlexboxLayout class="buttons" col="5" colSpan="2" row="0">
                            <button text="<" class="button__left" @tap="last()" col="5" row="0"/>
                            <button text=">" class="button__right" @tap="next()" col="6" row="0"/>
                        </FlexboxLayout>

                    </FlexboxLayout>
                </FlexboxLayout> 
        
                <FlexboxLayout class="weeks" col="0" colSpan="7" row="1">
                    <label text="Пн" row="1" col="0" class="week"/>
                    <label text="Вт" row="1" col="1" class="week"/>
                    <label text="Ср" row="1" col="2" class="week"/>
                    <label text="Чт" row="1" col="3" class="week"/>
                    <label text="Пт" row="1" col="4" class="week"/>
                    <label text="Сб" row="1" col="5" class="week"/>
                    <label text="Вс" row="1" col="6" class="week"/>
                </FlexboxLayout>

                <FlexboxLayout class="days" col="0" colSpan="7" row="2" rowSpan="6">
                    <template v-for="(day, key) in days">

                         <template v-if="day >= 23 && key <= 6">  <!-- Если мы не дошли до 6 итерации и число больше 23 -->
                            <FlexboxLayout :key="key">
                                <label :text="day" class="day_last"/>
                            </FlexboxLayout>
                        </template>

                        <template v-else-if="key > 18 && day <= 6"> <!-- Если у нас больше 18 итераций и число меньше 6 -->
                            <FlexboxLayout :key="key">
                                <label :text="day" class="day_last"/>
                            </FlexboxLayout>
                        </template>

                        <template v-else>

                            <template v-if="day == day_now && month_num_now == month_num && year_now == year">
                                <FlexboxLayout :key="key">
                                    <label :text="day" class="day_now"/>
                                </FlexboxLayout>
                            </template>

                            <template v-else>
                                <FlexboxLayout :key="key">
                                    <label :text="day" class="day"/>
                                </FlexboxLayout>
                            </template>
                            
                        </template>
                    </template>
                </FlexboxLayout>

                <FlexboxLayout class="back" col="4" colSpan="3" row="8">
                    <template v-if="month_num != month_num_now || year != year_now">
                        <button text="Текущий месяц" class="button__back" @tap="back()"/>
                    </template>
                </FlexboxLayout>
            </GridLayout>
        </ScrollView>
    </Page>
</template>

<script>
  export default {

    data() {
        return {
            month: "",
            month_num: 0,
            year: "",
            days: [],
            day_now: 0,
            month_num_now: 0,
            year_now: 0, // текущий год 
        };
    },

    mounted() {
        let current = new Date();
        this.year = current.getFullYear(); // получаю текущий год
        this.month_num = current.getMonth() + 1; // получаю текущий месяц (число)
        this.month = this.get_month(this.month_num); // получаю текущий месяц (строка)
        this.get_days(this.year, this.month_num); // заполняю дни месяца
        this.day_now = current.getDate(); // получаю текущее число
        this.month_num_now = this.month_num; // текущий месяц
        this.year_now = this.year; // текущий год
    },

    methods: {

        // предыдущий месяц
        last() { 
            if (this.month_num != 1) {
                this.month_num -= 1;
                this.month = this.get_month(this.month_num);
            }
            else {
                this.year -= 1;
                this.month_num = 12;
                this.month = this.get_month(this.month_num);
            }
            this.get_days(this.year, this.month_num);
        },

        // следующий месяц
        next() {
            if (this.month_num != 12) {
                this.month_num += 1;
                this.month = this.get_month(this.month_num);
            }
            else {
                this.year += 1;
                this.month_num = 1;
                this.month = this.get_month(this.month_num);
            }
            this.get_days(this.year, this.month_num);
        },

        // из числа месяца в название
        get_month(month_number) {
            let month_str = "";
            if (month_number == 1) {
                month_str = "Январь";
            }
            else if (month_number == 2) {
                month_str = "Февраль";
            }
            else if (month_number == 3) {
                month_str = "Март";
            }
            else if (month_number == 4) {
                month_str = "Апрель";
            }
            else if (month_number == 5) {
                month_str = "Май";
            }
            else if (month_number == 6) {
                month_str = "Июнь";
            }
            else if (month_number == 7) {
                month_str = "Июль";
            }
            else if (month_number == 8) {
                month_str = "Август";
            }
            else if (month_number == 9) {
                month_str = "Сентябрь";
            }
            else if (month_number == 10) {
                month_str = "Октябрь";
            }
            else if (month_number == 11) {
                month_str = "Ноябрь";
            }
            else {
                month_str = "Декабрь";
            }
            return month_str;
        },

        // получить дни месяца
        get_days(year, month) {
            this.days = [];

            // Первый день недели в выбранном месяце 
            var d = new Date(`${year}-${month}-1`);
            var weekday = d.getDay(); // получаю день недели (0 - вск, 1 - пн, 2 - вт ... 6 - сб)

            if (weekday == 1) { // если месяц начинается с понедельника
                for (let i = 1; i <= this.check_days(month, year); i++) {
                    this.days.push(i);
                }
            }
            else if (weekday == 2) { // если месяц начинается с вторника
                // добавляю в начало последнее число предыдущего месяца
                if(month != 1){
                    this.days.push(this.check_days(month-1, year));
                }
                else{
                    this.days.push(this.check_days(12, year-1));
                }
                
                for (let i = 1; i <= this.check_days(month, year); i++) {
                    this.days.push(i);
                }
            }
            else if (weekday == 3) { // если месяц начинается со среды
                // добавляю в начало последние числа предыдущего месяца (далее везде так)
                if(month != 1){
                    this.days.push(this.check_days(month-1, year) - 1);
                    this.days.push(this.check_days(month-1, year));
                }
                else{
                    this.days.push(this.check_days(12, year-1) - 1);
                    this.days.push(this.check_days(12, year-1));
                }
                
                for (let i = 1; i <= this.check_days(month, year); i++) {
                    this.days.push(i);
                }
            }
            else if (weekday == 4) { // если месяц начинается с четверга
                if(month != 1){
                    this.days.push(this.check_days(month-1, year) - 2);
                    this.days.push(this.check_days(month-1, year) - 1);
                    this.days.push(this.check_days(month-1, year));
                }
                else{
                    this.days.push(this.check_days(12, year-1) - 2);
                    this.days.push(this.check_days(12, year-1) - 1);
                    this.days.push(this.check_days(12, year-1));
                }

                for (let i = 1; i <= this.check_days(month, year); i++) {
                    this.days.push(i);
                }
            }
            else if (weekday == 5) { // если месяц начинается с пятницы
                if(month != 1){
                    this.days.push(this.check_days(month-1, year) - 3);
                    this.days.push(this.check_days(month-1, year) - 2);
                    this.days.push(this.check_days(month-1, year) - 1);
                    this.days.push(this.check_days(month-1, year));
                }
                else{
                    this.days.push(this.check_days(12, year-1) - 3);
                    this.days.push(this.check_days(12, year-1) - 2);
                    this.days.push(this.check_days(12, year-1) - 1);
                    this.days.push(this.check_days(12, year-1));
                }

                for (let i = 1; i <= this.check_days(month, year); i++) {
                    this.days.push(i);
                }
            }
            else if (weekday == 6) { // если месяц начинается с субботы
                if(month != 1){
                    this.days.push(this.check_days(month-1, year) - 4);
                    this.days.push(this.check_days(month-1, year) - 3);
                    this.days.push(this.check_days(month-1, year) - 2);
                    this.days.push(this.check_days(month-1, year) - 1);
                    this.days.push(this.check_days(month-1, year));
                }
                else{
                    this.days.push(this.check_days(12, year-1) - 4);
                    this.days.push(this.check_days(12, year-1) - 3);
                    this.days.push(this.check_days(12, year-1) - 2);
                    this.days.push(this.check_days(12, year-1) - 1);
                    this.days.push(this.check_days(12, year-1));
                }

                for (let i = 1; i <= this.check_days(month, year); i++) {
                    this.days.push(i);
                }
            }
            else if (weekday == 0) { // если месяц начинается с воскресенья
                if(month != 1){
                    this.days.push(this.check_days(month-1, year) - 5);
                    this.days.push(this.check_days(month-1, year) - 4);
                    this.days.push(this.check_days(month-1, year) - 3);
                    this.days.push(this.check_days(month-1, year) - 2);
                    this.days.push(this.check_days(month-1, year) - 1);
                    this.days.push(this.check_days(month-1, year));
                }
                else{
                    this.days.push(this.check_days(12, year-1) - 5);
                    this.days.push(this.check_days(12, year-1) - 4);
                    this.days.push(this.check_days(12, year-1) - 3);
                    this.days.push(this.check_days(12, year-1) - 2);
                    this.days.push(this.check_days(12, year-1) - 1);
                    this.days.push(this.check_days(12, year-1));
                }

                for (let i = 1; i <= this.check_days(month, year); i++) {
                    this.days.push(i);
                }
            }

            // Последний день недели в выбранном месяце 
            var d = new Date(`${year}-${month}-${this.check_days(month, year)}`);
            var weekday = d.getDay(); 

            if (weekday == 1) { // если месяц заканчивается в понедельник
                // добавляю в конец следующие числа месяца (везде так)
                this.days.push(1, 2, 3, 4, 5, 6);
            }
            else if (weekday == 2) { // если месяц заканчивается во вторник
                this.days.push(1, 2, 3, 4, 5);
            }
            else if (weekday == 3) { // если месяц заканчивается в среду
                this.days.push(1, 2, 3, 4);
            }
            else if (weekday == 4) { // если месяц заканчивается в четверг
                this.days.push(1, 2, 3);
            }
            else if (weekday == 5) { // если месяц заканчивается в пятницу
                this.days.push(1, 2);
            }
            else if (weekday == 6) { // если месяц заканчивается в субботу
                this.days.push(1);
            }
        },

        // проверка количества дней в месяце
        check_days(month_number, year) {
            if (month_number == 1 || month_number == 3 || month_number == 5 || month_number == 7 || month_number == 8 || month_number == 10 || month_number == 12) {
                return 31;
            }
            else if (month_number == 4 || month_number == 6 || month_number == 9 || month_number == 11) {
                return 30;
            }
            else { // февраль
                if (year % 4 === 0) { // если это високосный год
                    return 29;
                }
                else {
                    return 28;
                }
            }
        },

        // возврат к текущему месяцу
        back(){
            this.month_num = this.month_num_now;
            this.month = this.get_month(this.month_num);
            this.year = this.year_now;
            this.get_days(this.year, this.month_num);
        },
    },
};
</script>

<style scoped lang="scss">
    @import '@nativescript/theme/scss/variables/blue';

    actionbar {
        background-color: rgb(255, 255, 255);
    }

    .header {
        color: rgb(0, 0, 0);
        font-size: 26px;
    }

    page {
        background-color: rgb(255, 255, 255);
    }

    .flexbox{
        display: flex;
        flex-direction: column;
        height: auto;
        margin: 20px;
    }

    .month{
        display: flex;
        justify-content: space-between;
        align-items: center;
        
    }

    .label__month{
        font-size: 24px;
        color: black;
    }

    .label__year{
        font-size: 24px;
        margin-left: 15px;
        color: black;
    }

    .button__left{
        border-radius: 50px;
        width: 20px;
        height: 100px;
        font-size: 26px;
        background-color: rgb(235, 235, 235);
        color: black;
    }

    .button__right{
        border-radius: 50px;
        width: 20px;
        height: 100px;
        font-size: 26px;
        background-color: rgb(235, 235, 235);
        color: black;
    }

    .days{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        align-items: flex-start;
        justify-content: flex-start;
        margin: 20px;
        height: auto;
    }

    .day{
        font-size: 24px;
        width: 13.5%;
        background-color: rgb(230, 230, 230);
        margin-left: 2px;
        margin-right: 2px;
        margin-top: 5px;
        text-align: center;
        height: auto;
        color: black;
    }

    .day_now{
        font-size: 24px;
        width: 13.5%;
        background-color: rgb(202, 252, 197);
        margin-left: 2px;
        margin-right: 2px;
        margin-top: 5px;
        text-align: center;
        height: auto;
        color: black;
    }

    .day_last{
        font-size: 24px;
        width: 13.5%;
        background-color: rgb(246, 246, 246);
        margin-left: 2px;
        margin-right: 2px;
        margin-top: 5px;
        text-align: center;
        height: auto;
        color: rgb(190, 190, 190);
    }

    .weeks{
        margin: 20px;
        height: auto;
    }

    .week{
        font-size: 24px;
        width: 13.8%;
        text-align: center;
        color: black;
    }

    .back{
        margin: 20px;
    }

    .button__back{
        border-radius: 50px;
        width: auto;
        height: auto;
        font-size: 18px;
        background-color: rgb(235, 235, 235);
        color: black;
    }

</style>
