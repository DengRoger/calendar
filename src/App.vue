<template>
<div class="d-flex flex-column justify-content-start m-0" style="height: 10vh;">
  <!--上10-->
</div>
<div class="row" style="width: 100%; height: 90vh;">
  <div class="col" style="max-width: 20%;">
    <!--左20%-->
  </div>
  <div class="d-flex col" style="min-width: 80%">
    <div class=" d-none d-sm-block" style="width: 100%; height: 100%; ">
      <div class="row">
        <div v-for="Day in 7" :key="Day" class="col panel" style="display: block;">
          {{days[Day-1]}}
        </div>
      </div>
      <div v-for="Week in 5" :key="Week" class="row" style=" height: 19% ">
        <div v-for="Day in 7" :key="Day" class="col panel border" style="display: block; Height: 19vh">
          {{(Week - 1)*7 + Day - 1}}
        </div>
      </div>
    </div> 
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      // [上個月結束時間點、下個月開始點]
      days: ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'],
      currentDate: '',
      year: '',
      month: '',
      date: '',
      firstDay: '',
      lastMonthDays: '',
      weekFirstDay: '',
      weekLastDay: '',
    };
  },
  mounted() {
    this.getDate();
    this.lastDayOfLastMonth();
  },
  methods: {
    getLastMonthDays() {
      let month = this.month;
      let year = this.year;
      if(this.month == 0)
        month = 11, year = this.year - 1;
      else month = this.month - 1; 
      console.log(month);
      console.log(year);
      this.lastMonthDays = new Date(year, month, 0).getDate();
    },
    getDate() {
      this.currentDate = new Date();
      this.year = this.currentDate.getFullYear();
      this.month = this.currentDate.getMonth() + 1;
      this.date = this.currentDate.getDate();
    },
    firstDayOfMonth() {
      this.firstDay = new Date(this.year, this.month - 1, 1);
    },
    lastDayOfLastMonth() {
      this.getLastMonthDays();
    },
    getWeekFirstDay() {
      this.weekFirstDay = this.firstDay.getDay();
      //this.lastMonthDays = new Date(this.year, this.month - 1, 0).getDate();
      this.getLastMonthDays();
      // if this.month == 0 then this.month = 12 
      // else this.month = this.month - 1
    },
    getWeekLastDay() {
      this.weekLastDay = new Date(this.year, this.month, 0).getDay();
    },
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
}
</style>
