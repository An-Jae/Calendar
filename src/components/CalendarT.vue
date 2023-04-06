<template>
  <div class="calendar">
    <div class="header">
      <button @click="prevMonth">&lt;</button>
      <h2>{{ year }}年 {{ month }}月</h2>
      <button @click="nextMonth">&gt;</button>
    </div>
    <table>
      <thead>
        <tr>
          <th>日</th>
          <th>月</th>
          <th>火</th>
          <th>水</th>
          <th>木</th>
          <th>金</th>
          <th>土</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="week in weeks" :key="week">
          <td v-for="day in week" :key="day.date"
            :class="{ today: day.today, 'not-this-month': !day.inThisMonth }"
            @click="selectDate(day.date)">{{day.date}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    const now = new Date();
    return {
      year: now.getFullYear(),
      month: now.getMonth() + 1,
      selectedDate: null,
    };
  },
  computed: {
    weeks() {
      const firstDayOfMonth = new Date(this.year, this.month - 1, 1);
      const lastDayOfMonth = new Date(this.year, this.month, 0);
      const daysInMonth = lastDayOfMonth.getDate();
      const firstDayOfWeek = firstDayOfMonth.getDay();
      // const lastDayOfWeek = lastDayOfMonth.getDay();
      const weeks = [[]];
      let day = 1;
      for (let i = 0; i < 7; i++) {
        if (i < firstDayOfWeek || day > daysInMonth) {
          weeks[0].push({ date: null, inThisMonth: false });
        } else {
          weeks[0].push({ date: new Date(this.year, this.month - 1, day), inThisMonth: true, today: this.isToday(day) });
          day++;
        }
      }
      let currentWeek = 0;
      while (day <= daysInMonth) {
        if (weeks[currentWeek].length === 7) {
          currentWeek++;
          weeks.push([]);
        }
        weeks[currentWeek].push({ date: new Date(this.year, this.month - 1, day), inThisMonth: true, today: this.isToday(day) });
        day++;
      }
      while (weeks[currentWeek].length < 7) {
        weeks[currentWeek].push({ date: null, inThisMonth: false });
      }
      return weeks;
    },
    _getDate:function(d){
      return d.date;
    },
  },
  methods: {
    prevMonth() {
      if (this.month === 1) {
        this.year--;
        this.month = 12;
      } else {
        this.month--;
      }
    },
    nextMonth() {
      if (this.month === 12) {
        this.year++;
        this.month = 1;
      } else {
        this.month++;
      }
    },
    selectDate(date) {
      this.selectedDate = date;
    },
    isToday(day) {
      const today = new Date();
      return this.year === today.getFullYear() && this.month - 1 === today.getMonth() && day === today.getDate();
},
},
};
</script>

<style scoped>
.calendar {
  font-size: 14px;
  margin: 20px auto;
  max-width: 400px;
}

h2 {
  width: 100%;
  height: 50%;
  background-color: white;
  white-space: nowrap;
  overflow: hidden;
  align-items: center;
}

.header {
  background-color: white;
  overflow: auto;
  display: flex;
  justify-content: space-between;
  align-items: right;
}

table {
  width: 100%;
  border-collapse: collapse;
  border-spacing: 0;
}

td {
  padding: 10px;
  text-align: center;
  border: 1px solid #ccc;
  cursor: pointer;
}

.today {
  background-color: #eee;
}

.not-this-month {
  color: #ccc;
}
</style>
