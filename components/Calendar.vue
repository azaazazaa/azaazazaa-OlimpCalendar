<template>
<div>


  <div class="calendar_wrapper">
    <header class="calendar_header">
      <button class="button arrow-left absolute left" @click="changeMonth(0)"></button>
      <h1>{{monthList[currentMonthId]}} {{currentYear}}</h1>
      <button class="button arrow-right absolute right" @click="changeMonth(1)"></button>
    </header>
    <main class="calendar">
      <table>
        <thead>
          <tr>
            <th 
              v-for="(value, index) in weekList"
              :key="index"
            >
            {{value}}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="w in 7"
            :key="w"
          >
            <td
              v-bind:data-week="w"
              v-for="i in 7"
              :key="i"
              @click="addEvent(i, w)"
            >
            {{fillDays(i,w)}}
            </td>
          </tr>
        </tbody>
      </table>
    </main>
  </div>
  <InputEvent 
    :date="[currentDateOfMonth,
            currentMonthId,
            currentYear]"

  />
</div>
</template>

<script>
export default {
  data(){
    return{
      weekList:[
        'ПН',
        'ВТ',
        'СР',
        'ЧТ',
        'ПТ',
        'СБ',
        'ВС'
      ],
      monthList:[
        'Январь',
        'Февраль',
        'Март',
        'Апрель',
        'Май',
        'Июнь',
        'Июль',
        'Август',
        'Сенятбрь',
        'Октябрь',
        'Ноябрь',
        'Декабрь'
      ],
      currentMonthId: 11,
      currentYear: 2020,
      currentDateOfMonth: -1,
    }
  },
  methods:{
    fillDays(dayOfWeek, numberOfWeek){
      let currentMonth = new Date(this.currentYear, this.currentMonthId);
      let currentDate = new Date(this.currentYear, this.currentMonthId, dayOfWeek+((numberOfWeek-1)*7) );
      
      currentMonth.daysInPreviousMonth = function() {
		    return 33 - new Date(this.getFullYear(), this.getMonth() - 1, 33).getDate();
      };
      
      if(currentMonth.getDay() !== 1){ // если 1 число не выпадает на понедельник
        let currentDateAnother = new Date(this.currentYear, 
          this.currentMonthId - 1, 
          currentMonth.daysInPreviousMonth() - currentMonth.getDay() + dayOfWeek + ( ( numberOfWeek - 1 ) * 7 ) + 1
        );
        return currentDateAnother.getDate()
      }
      
      return currentDate.getDate()
    },
    changeMonth(arg){
      if(arg==0){
        if(this.currentMonthId - 1 < 0){
          this.currentMonthId = 11;
          this.currentYear = this.currentYear - 1;
        }else{
          this.currentMonthId = this.currentMonthId - 1;
        }
      }else{
        if(this.currentMonthId + 1 > 11){
          this.currentMonthId = 0
          this.currentYear = this.currentYear + 1;
        } else{
          this.currentMonthId = this.currentMonthId + 1;
        }
      }
    },
    addEvent(dayOfWeek, numberOfWeek){
      this.currentDateOfMonth = this.fillDays(dayOfWeek, numberOfWeek);
    }
  }
}
</script>

<style lang="scss" scoped>
  $width: 800px;
  $height: $width / 2.133;
  .calendar{
    &_wrapper{
      width: 800px;
      height: 400px;
      margin-left: auto;
      margin-right: auto;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: center;
    }
    &_header{
      position: relative;
      width: $width;
      display: inline-block;
      text-align: left;
      padding-left: $width/14;
      margin: 20px 0;
    }
    table {
      border: 1px solid grey;
      border-collapse: collapse;  
      width: $width;
      height: $height;
    } 
    th {
      border: 1px solid grey;
      width: $width/7;
      height: $height/7;
    }

    td {
      border: 1px solid grey;
      width: $width/7;
      height: $height/7;
    }
  }
  
</style>
