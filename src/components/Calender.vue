<template>
  <div>
    <h2>Calender</h2>
    <h3>{{date.getFullYear()}}年 {{date.getMonth() + 1}}月</h3>
    <button @click="prevMonth">Prev</button>
    <button @click="nextMonth">Next</button>
    <table>
      <tr>
        <th>S</th>
        <th>M</th>
        <th>T</th>
        <th>W</th>
        <th>T</th>
        <th>F</th>
        <th>S</th>
      </tr>
      <tr v-for="(week, i) in this.calenderArray" :key="i">
        <td
          v-for="(day, j) in week"
          :key="j"
          @click="onDayClick"
        >{{day}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      year: 0,
      month: 0
    }
  },
  created: function() {
    const now = new Date();
    this.year = now.getFullYear();
    this.month = now.getMonth();
  },
  computed: {
    date: function() {
      return new Date(this.year, this.month);
    },
    firstDay: function() {
      const firstDayDate = new Date(this.date.getTime());
      firstDayDate.setDate(1);
      return firstDayDate.getDay();
    },
    calenderArray: function() {
      const lastDay = new Date(this.date.getFullYear(), this.date.getMonth() + 1, 0).getDate();
      let currentDay = 0;
      return [...Array(5)].map((week, i) => {
        return [...Array(7)].map((day, j) => {
          if (i == 0) {
            // 1週目は開始の曜日までスキップする
            if (j < this.firstDay) {
              return '';
            }
          }

          currentDay += 1;
          if (currentDay <= lastDay) {
            return currentDay;
          }
        })
      })
    }
  },
  methods: {
    prevMonth: function() {
      this.month -= 1;
    },
    nextMonth: function() {
      this.month += 1;
    },
    onDayClick: function(event) {
      console.log(event.target.innerText);
    }
  }
}
</script>

<style scoped>
  table {
    margin: auto;
  }

  td {
    padding: 4px;
  }
</style>

