<template>
  <div id="pneko">
    <datepicker class="date-input" :disabled="dateDisabled" :monday-first="true" :placeholder="'請選擇日期'" :format="'yyyy-MM-dd'" v-model="selectedDate"></datepicker>
    <select class="time-select" v-model="selectedTime">
      <option value="">請選擇時間</option>
      <option v-for="time in times" :value="time">{{ timeFormat(time) }}</option>
    </select>
    <div class="show">
      <p>選擇日期：{{ stringDate }}</p>
      <p>選擇時間：{{ selectedTime }}</p>
    </div>
  </div>
</template>

<script>
import datepicker from 'vuejs-datepicker'
import fakeData from './data'

export default {
  name: 'app',
  components: {
    datepicker
  },
  data () {
    return {
      dateDisabled: {
        to: new Date(2017, 10, 1),
        from: new Date(2017, 10, 30),
        days: [1],
        dates: [
          new Date(2017, 10, 10),
          new Date(2017, 10, 15)
        ]
      },
      times: [],
      selectedDate: '',
      selectedTime: ''
    }
  },
  computed: {
    stringDate () {
      let string = ''
      if (this.selectedDate !== '') {
        let year = this.selectedDate.getFullYear()
        let month = this.selectedDate.getMonth() + 1
        let date = this.selectedDate.getDate()
        string = [
          year,
          (month > 9 ? '' : '0') + month,
          (date > 9 ? '' : '0') + date
        ].join('-')
      }
      return string
    }
  },
  watch: {
    'stringDate' () {
      this.getApiTimes()
    }
  },
  methods: {
    getApiTimes () {
      let json = fakeData.filter(object => {
        if (object.date === this.stringDate) {
          return object
        }
      })[0]
      this.times = json.times
      this.selectedTime = ''
    },
    timeFormat (military) {
      let time = military.split(':')
      let hour = Number(time[0])
      let minute = time[1]
      let added = ' AM'
      if (hour > 12) {
        added = ' PM'
        hour -= 12
      }
      return hour + ':' + minute + added
    }
  }
}
</script>

<style>
#pneko{
  font-size: 14px;
}

.date-input input{
  padding: 5px 7px;
  font-size: 14px;
  width: 200px;
  border: 1px solid #ddd;
}

.time-select{
  margin-top: 20px;
  font-size: 14px;
  width: 200px;
  background: transparent;
  border-radius: 0;
  padding: 5px 7px;
  -webkit-appearance: none;
  border: 1px solid #ddd;
  height: 28px;
}
</style>
