<template>
  <div id="calendar">
    <!-- 年份 月份 -->
    <div class="head">
      <div class="date">
        <p class="month">{{(new Date().getDate() + '').length === 1 ? '0' + new Date().getDate() : new Date().getDate()
          + ''}}</p>
        <p>{{currentYear}}-{{currentMonth}}</p>
      </div>
      <div class="detail">
        <p class="total">今天共0节课</p>
        <p>普通课0节/试听课0节/活动课0节</p>
      </div>
      <div class="control">
        <span class="last-month" @click="pickPre(currentYear,currentMonth)">
          ❮
        </span>
        <span class="next-month" @click="pickNext(currentYear,currentMonth)">
          ❯
        </span>
      </div>
    </div>
    <!-- 星期 -->
    <ul class="weekdays">
      <li>一</li>
      <li>二</li>
      <li>三</li>
      <li>四</li>
      <li>五</li>
      <li>六</li>
      <li>日</li>
    </ul>
    <!-- 日期 -->
    <ul class="days">
      <!-- 核心 v-for循环 每一次循环用<li>标签创建一天 -->
      <li v-for="(dayobject, index) in days" :key="index">
        <!--本月-->
        <!--如果不是本月  改变类名加灰色-->

        <span v-if="dayobject.day.getMonth()+1 != currentMonth" class="other-month">
          <span>
            {{ dayobject.day.getDate() }}
          </span>
        </span>

        <!--如果是本月  还需要判断是不是这一天-->
        <span v-else>
          <!--今天  同年同月同日-->
                <span
                  v-if="dayobject.day.getFullYear() == new Date().getFullYear() && dayobject.day.getMonth() == new Date().getMonth() && dayobject.day.getDate() == new Date().getDate()"
                  class="active">{{ dayobject.day.getDate() }}</span>
                <span v-else
                      class="has-data">{{ dayobject.day.getDate() }}</span>
            </span>

      </li>
    </ul>
  </div>
</template>
<script>
  export default {
    name: 'DatePicker',
    data() {
      return {
        currentDay: 1,
        currentMonth: 1,
        currentYear: 1970,
        currentWeek: 1,
        days: []
      }
    },
    created: function () {  //在vue初始化时调用
      this.initData(null);
    },
    methods: {
      initData: function (cur) {
        // 存放剩余数量
        // var leftcount=0;
        var date;
        if (cur) {
          date = new Date(cur);
        } else {
          let now = new Date();
          let d = new Date(this.formatDate(now.getFullYear(), now.getMonth(), 1));
          d.setDate(35);
          date = new Date(this.formatDate(d.getFullYear(), d.getMonth() + 1, 1));
        }
        this.currentDay = date.getDate();
        this.currentYear = date.getFullYear();
        this.currentMonth = (date.getMonth() + 1) + '';
        this.currentMonth = this.currentMonth.length === 1 ? '0' + this.currentMonth : this.currentMonth

        this.currentWeek = date.getDay(); // 1...6,0
        if (this.currentWeek == 0) {
          this.currentWeek = 7;
        }
        let str = this.formatDate(this.currentYear, this.currentMonth, this.currentDay);
        this.days.length = 0;
        // 今天是周日，放在第一行第7个位置，前面6个
        // 初始化本周
        for (let i = this.currentWeek - 1; i >= 0; i--) {
          let d = new Date(str);
          d.setDate(d.getDate() - i);
          let dayobject = {}; // 用一个对象包装Date对象  以便为以后预定功能添加属性
          dayobject.day = d;
          this.days.push(dayobject);// 将日期放入data 中的days数组 供页面渲染使用
        }
        //其他周
        for (let i = 1; i <= 35 - this.currentWeek; i++) {
          let d = new Date(str);
          d.setDate(d.getDate() + i);
          let dayobject = {};
          dayobject.day = d;
          this.days.push(dayobject);
        }
      },
      pickPre: function (year, month) {
        // setDate(0); 上月最后一天
        // setDate(-1); 上月倒数第二天
        // setDate(dx) 参数dx为 上月最后一天的前后dx天
        let d = new Date(this.formatDate(year, month, 1));
        d.setDate(0);
        this.initData(this.formatDate(d.getFullYear(), d.getMonth() + 1, 1));
      },
      pickNext: function (year, month) {
        let d = new Date(this.formatDate(year, month, 1));
        d.setDate(35);
        this.initData(this.formatDate(d.getFullYear(), d.getMonth() + 1, 1));
      },
      // 返回 类似 2016-01-02 格式的字符串
      formatDate: function (year, month, day) {
        let y = year;
        let m = month;
        if (m < 10) m = "0" + m;
        let d = day;
        if (d < 10) d = "0" + d;
        return y + "-" + m + "-" + d
      }
    }
  }
</script>
<style scoped>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  ul {
    list-style-type: none;
  }

  #calendar {
    width: 50%;
    display: inline-block;
    box-shadow: 0 1px 50px #ccc;
  }

  .head {
    width: 100%;
    height: 80px;
    background: #fff;
    display: flex;
    text-align: left;
    align-items: center;
  }

  .head .date {
    width: 120px;
    padding: 0 20px;
    font-size: 20px;
    border-right: 1px solid #ccc;
  }

  .head .date .month {
    font-size: 35px;
  }

  .head .detail {
    flex-grow: 1;
    padding-left: 20px;
  }

  .head .detail p {
    font-size: 16px;
  }

  .head .detail .total {
    font-size: 20px;
    padding-bottom: 5px;
  }

  .head .control {
    width: 130px;
    display: flex;
    justify-content: space-between;
    padding-right: 20px;
  }

  .head .control .last-month {
    padding: 5px 20px;
    border: 1px solid #ccc;
    border-radius: 20px;
    cursor: pointer;
  }

  .head .control .last-month:hover {
    background: #ddd;
  }

  .head .control .next-month {
    padding: 5px 20px;
    border: 1px solid #ccc;
    border-radius: 20px;
    cursor: pointer;
  }

  .head .control .next-month:hover {
    background: #ddd;
  }

  .weekdays {
    margin: 0;
    padding: 10px 0;
    background-color: #f6fdfb;
    display: flex;
    flex-wrap: wrap;
    color: #000;
    justify-content: space-around;
  }

  .weekdays li {
    display: inline-block;
    width: 13.6%;
    text-align: center;
  }

  .days {
    padding: 20px 0;
    background: #FFFFFF;
    margin: 0;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }

  .days li {
    list-style-type: none;
    display: inline-block;
    padding-bottom: 15px;
    width: 14.2%;
    text-align: center;
    font-size: 1rem;
    color: #000;
  }

  .days li span span {
    display: inline-block;
    width: 35px;
    height: 35px;
    line-height: 35px;
    text-align: center;
    border-radius: 50%;
    background: #fff;
    cursor: pointer;
  }

  .days li span span:hover {
    background: #ddd;
  }

  .days li .has-data {
    position: relative;
  }

  .days li .has-data:after {
    display: inline-block;
    content: " ";
    width: 3px;
    height: 3px;
    background: #EF4C4F;
    position: absolute;
    bottom: -7px;
    left: 50%;
    margin-left: -1px;
  }

  .days li .active {
    background: #EF4C4F;
    color: #fff;
  }

  .days li .active:hover {
    background: #EF4C4F;
  }

  .days li .other-month {
    padding: 5px;
    color: gainsboro;
  }
</style>
