<template>
<div class="row m-0 align-items-center" style="height: 10vh;">
  <!--上10 set two button calling func monthAdd , monthSub set these buttons at one line -->
  <div class="col" style="max-width: 20%;">
    <!-- 上10左20 -->
  </div>
  <div class="col" style="max-width: 10%;">
    <div class="row m-0">
      <div class="col p-0 border-0">
        <button type="button" class="btn btn-light border-0"  @click="today">today</button>
      </div>
      <div class="col p-0">
        <button type="button" class="btn btn-light rounded-circle border-0" @click="monthSub">&lt;</button>
      </div>
      <div class="col p-0 border-0 p-0">
        <button type="button" class="btn btn-light rounded-circle border-0" @click="monthAdd">&gt;</button>
      </div>
    </div>
  </div>
  <div class="col" style="max-width: 20%;">
    <div class="col d-flex align-items-center p-0">
      <h3 style="margin: 0%;">{{year}}/{{month + 1}}</h3>
    </div>
  </div>
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
      <div v-for="Week in 6" :key="Week" class="row" style=" height: 16% ">
        <div v-for="Day in 7" :key="Day" class="col panel border" style="display: block; Height: 19vh">
          <span v-if="checkIsToday((Week - 1) * 7 + Day)" style="background-color: rgb(35, 166, 210); color: white; border-radius: 30%;">{{caculateMonthVal((Week - 1)*7 + Day)}}</span>
          <span v-else>{{ caculateMonthVal((Week - 1) * 7 + Day) }}</span>
        </div>
      </div>
    </div> 
  </div>
</div>
</template>

<script>
/*
  前端資料:
    days: ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT']
  後端資料:
    currentDate: 現在時間
    year:  年
    month: 月
    date:  日
    monthFirstDay_DayOfWeek: 月的第一天是星期幾
    lastMonthDays: 上個月有幾天
    thisMonthDays: 這個月有幾天
    weekFirstDay = monthFirstDay_DayOfWeek: 這個月的第一天是星期幾

  初始化：
    getCurrentDate (以月展示) : 同步更新 year, month, date
    getMonthFirstDay (月的第一天是星期幾)
    getLastMonthDays (上個月有幾天)
    getThisMonthDays (這個月有幾天)
  
  若月份 +- :
    -:
      if month == 0
        year = year - 1
        month = 11
      else
        month = month - 1
    +:
      if month == 11
        year = year + 1
        month = 0
      else
        month = month + 1

    重新獲取資料：
      getMonthFirstDay (月的第一天是星期幾)
      getLastMonthDays (上個月有幾天)
      getThisMonthDays (這個月有幾天)
      update weekFirstDay = monthFirstDay_DayOfWeek: 這個月的第一天是星期幾
  
  週：
    +-:
      +:
        if weekFirstDay == 6
          weekFirstDay = 0
        else
          weekFirstDay = weekFirstDay + 1
  前端判段：
    mounthIndexReturn func(index):
      將傳進 index -> ((Week - 1)*7 + Day) 
      if index < monthFirstDay_DayOfWeek
        return lastMonthDays - monthFirstDay_DayOfWeek + (Week - 1)*7 + Day
      else if index > monthFirstDay_DayOfWeek && index <= thisMonthDays + monthFirstDay_DayOfWeek
        return (Week - 1)*7 + Day - monthFirstDay_DayOfWeek
      else
        return (Week - 1)*7 + Day - monthFirstDay_DayOfWeek - thisMonthDays
*/
export default {
  data() {
    return {
      days: ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'],
      currentDate: new Date(),
      year: 0,
      month: 0,
      date: 0,
      monthFirstDay_DayOfWeek: 0,
      lastMonthDays: 0,
      thisMonthDays: 0,
      weekFirstDay: 0,
    };
  },
  mounted() {
    this.getCurrentDate();
    this.getMonthFirstDay();
    
    this.getLastMonthDays();
    this.getThisMonthDays();
    window.alert = function() {
      return false;
    };
  },
  methods: {
    getCurrentDate() {
      this.year = this.currentDate.getFullYear();
      this.month = this.currentDate.getMonth();
      this.date = this.currentDate.getDate();
      console.log(this.year);
    },
    getMonthFirstDay() {
      this.monthFirstDay_DayOfWeek = new Date(this.year, this.month, 1).getDay();
      this.weekFirstDay = this.monthFirstDay_DayOfWeek;
    },
    getLastMonthDays() {
      this.lastMonthDays = new Date(this.year, this.month, 0).getDate();
    },
    getThisMonthDays() {
      this.thisMonthDays = new Date(this.year, this.month + 1, 0).getDate();
    },
    caculateMonthVal(index) {
      if (index <= this.monthFirstDay_DayOfWeek) {
        return this.lastMonthDays - this.monthFirstDay_DayOfWeek + index;
      } else if (index > this.monthFirstDay_DayOfWeek && index <= this.thisMonthDays + this.monthFirstDay_DayOfWeek) {
        return index - this.monthFirstDay_DayOfWeek;
      } else {
        return index - this.monthFirstDay_DayOfWeek - this.thisMonthDays;
      }
    },
    weekFirstDayAdd() {
      if (this.weekFirstDay == 6) {
        this.weekFirstDay = 0;
      } else {
        this.weekFirstDay = this.weekFirstDay + 1;
      }
    },
    weekFirstDaySub() {
      if (this.weekFirstDay == 0) {
        this.weekFirstDay = 6;
      } else {
        this.weekFirstDay = this.weekFirstDay - 1;
      }
    },
    monthAdd() {
      if (this.month == 11) {
        this.year = this.year + 1;
        this.month = 0;
      } else {
        this.month = this.month + 1;
      }
      this.getMonthFirstDay();
      this.getLastMonthDays();
      this.getThisMonthDays();
    },
    monthSub() {
      if (this.month == 0) {
        this.year = this.year - 1;
        this.month = 11;
      } else {
        this.month = this.month - 1;
      }
      this.getMonthFirstDay();
      this.getLastMonthDays();
      this.getThisMonthDays();
    },
    today() {
      this.getCurrentDate();
      this.getMonthFirstDay();
      this.getLastMonthDays();
      this.getThisMonthDays();
    },
    checkIsToday(index) {
      if (this.year == this.currentDate.getFullYear() && this.month == this.currentDate.getMonth() && this.date == this.caculateMonthVal(index)) {
        return true;
      } else {
        return false;
      }
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
.today-marker {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 30px;
  height: 30px;
  background-color: blue;
  border-radius: 50%;
}
</style>
