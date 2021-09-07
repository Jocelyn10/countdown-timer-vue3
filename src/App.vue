<template>
  <p>
    {{ diff.year }} years, {{ diff.month }} months, {{ diff.day }} days,
    {{ diff.hour }} hours, {{ diff.minute }} minute, {{ diff.second }} seconds
    until
    {{ formatDate(futureDate) }}
  </p>
</template>
<script>
const futureDate = new Date(2050, 0, 1); // This is our reference
const getDateDiff = (date1, date2) => {
  const diff = new Date(date2.getTime() - date1.getTime());
  return {
    year: diff.getUTCFullYear() - 1970,
    month: diff.getUTCMonth(),
    day: diff.getUTCDate() - 1,
    hour: diff.getUTCHours(),
    minute: diff.getUTCMinutes(),
    second: diff.getUTCSeconds(),
  };
};

export default {
  name: "App",
  data() {
    return {
      futureDate,
      diff: {},
      timer: undefined,
    };
  },
  methods: {
    getDiff() {
      this.diff = getDateDiff(new Date(), futureDate);
    },
    formatDate(date) {
      let d = new Date(date),
        month = (d.getMonth() + 1).toString(),
        day = d.getDate().toString(),
        year = d.getFullYear().toString();
      if (month.length < 2) month = "0" + month;
      if (day.length < 2) day = "0" + day;
      return [year, month, day].join("-");
    },
  },
  beforeMount() {
    // This line is called each minute
    this.timer = setInterval(this.getDiff, 1000);
  },
  beforeUnmount() {
    clearInterval(this.timer);
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
