<template>
<div class="row m-0 align-items-center" style="height: 10vh;">
  <!--上10 set two button calling func monthAdd , monthSub set these buttons at one line -->
  <div class="col" style="max-width: 20%;">
    <!-- 上10左20 -->
  </div>
  <div class="col" style="max-width: 10%; min-width: 200px;">
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
  <div class="col" style="max-width: 7%; min-width: 150px;">
    <div class="col d-flex align-items-center p-0">
      <h3 style="margin: 0%;">{{year}}/{{month + 1}}</h3>
    </div>
  </div>
  <div class="col" style="max-width: 40%;">
  </div>
  <div class="col" style="max-width: 20%;">
    <!-- 下拉式選單: year, month, week-->
    <select>
      <option value="option1">Year</option>
      <option value="option2" selected>Month</option>
      <option value="option3">Week</option>
    </select>
  </div>
</div>
<div class="row p-0" style="width: 100%; height: 90vh;">
  <div class="col p-0" style="max-width: 20%;">
    <!--左20%-->
  </div>
  <div class="d-flex col p-0" style="min-width: 80%">
    <div class=" d-none d-sm-block" style="width: 100%; height: 100%; ">
      <div class="row">
        <div v-for="Day in 7" :key="Day" class="col panel" style="display: block;">
          {{days[Day-1]}}
        </div>
      </div>
      <div v-for="Week in 6" :key="Week" class="row" style="height: 16%">
        <div v-for="Day in 7" :key="Day" :id=" ((Week - 1) * 7 + Day)" class="col panel border p-0" style="display: block Height: 19%"
          @mousedown="handleMouseDown(((Week - 1) * 7 + Day))"
          @mousemove="handleMouseMove(((Week - 1) * 7 + Day))"
          @mouseup="handleMouseUp(((Week - 1) * 7 + Day))">
          <span v-if="checkIsToday((Week - 1) * 7 + Day)" style="user-select: none; background-color: rgb(35, 166, 210); color: white; border-radius: 30%;">{{caculateMonthVal((Week - 1)*7 + Day)}}</span>
          <span v-else style="user-select: none;" >{{ caculateMonthVal((Week - 1) * 7 + Day) }}</span>
        </div>
      </div>
      <div v-for="(item, idx) in button" :key="idx">
        <button v-if="item[0]" type="button" class="btn btn-primary p-0" :style="{left: item[1] + 'px', top: item[2] + 'px', width: item[3] + 'px',  position: 'absolute'}">{{item[4]}}</button> 
      </div>
    </div>
  </div>
</div>
</template>
<script>
export default {
  data() {
    return {
      windowWidth: window.innerWidth,
      windowHeight: window.innerHeight,
      button: [[false,0,0,201,'test'],[false,524.3,0,201,'noTitle'],[false,0,0,0,'noTitle'],[false,0,0,0,'noTitle'],[false,0,0,0,'noTitle'],[false,0,0,0,'noTitle']],
      days: ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'],
      currentDate: new Date(),
      year: 0,
      month: 0,
      date: 0,
      monthFirstDay_DayOfWeek: 0,
      lastMonthDays: 0,
      thisMonthDays: 0,
      weekFirstDay: 0,
      mousedown : false,
      mousedownIndex: 0,
      mousemoveIndex: 0,
      mouseupIndex: 0,
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
    window.addEventListener('resize', () => {
      this.windowWidth = window.innerWidth;
      this.windowHeight = window.innerHeight;
      this.caculateButtonPosition();
    });
    this.caculateButtonPosition();
  },
  methods: {
    getCurrentDate() {
      this.year = this.currentDate.getFullYear();
      this.month = this.currentDate.getMonth();
      this.date = this.currentDate.getDate();
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
    handleMouseDown(index) {
      this.mousedown = true;
      this.mousedownIndex = index;
      this.mousemoveIndex = index;
      this.mouseupIndex = index;
    },
    handleMouseMove(index) {
      if (this.mousedown) {
        this.mousemoveIndex = index;
        this.controlButtonShow();
      }
    },
    handleMouseUp(index) {
      if (this.mousedown) {
        this.mousedown = false;
        this.mouseupIndex = index;
        this.mousemoveIndex = index;
        this.controlButtonHide();
      }
    },
    showButton(startIndex, endIndex) {

      this.caculateButtonPosition();
      let i ;

      let posD = Math.floor((startIndex-1)/7);
      let startLine = Math.floor((startIndex-1)/7);
      let endLine = Math.floor((endIndex-1)/7);

      let left = document.getElementById(startIndex).getBoundingClientRect().x;
      this.button[posD][1] = left;
      if (startLine == endLine){
        this.button[posD][3] = (endIndex - startIndex + 1) * 0.11784 * this.windowWidth;
      } else {
        let tmp1 = (startIndex - 1)% 7 + 1;
        let tmp2 = (endIndex - 1)% 7 + 1;
        this.button[posD][3] = this.windowWidth*0.11784* (7 - tmp1 + 1);
        for (let i = startLine + 1; i < endLine; i++){
          this.button[i][1] = document.getElementById(1).getBoundingClientRect().x;
          this.button[i][3] = this.windowWidth*0.11784*7;
        }
        this.button[endLine][1] = document.getElementById(1).getBoundingClientRect().x;
        this.button[endLine][3] = this.windowWidth*0.11784* (tmp2);
      }
      for (i = 0 ; i <= 5 ; i++){
        if( i >= Math.floor((startIndex-1)/7) && i <= Math.floor(endIndex-1)/7){              
          this.button[i][0] = true;
        } else this.button[i][0] = false;
      }
    },
    controlButtonShow() {
      if (this.mousedown) {
        
        if (this.mousemoveIndex > this.mousedownIndex){
          this.showButton(this.mousedownIndex, this.mousemoveIndex)
        } else {
          this.showButton(this.mousemoveIndex, this.mousedownIndex)

        }
      }
    },
    controlButtonHide(){
      for(let i = 0; i < 6; i++){
        this.button[i][0] = false;
      }
    },
    caculateButtonPosition(){
      for(let i = 0; i < 6; i++){
        if(this.button[i][0]){
          this.button[i][2] = (this.windowHeight*0.1 + 46) + (this.windowHeight*(0.14399*i));
        }
      }
    }
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