<template>
  <div class="app" :class="{'align-right': alignRight}">
    <h1>Examples</h1>
    <div class="buttons">
      <button @click="toggleDatepickers">Hide datepickers</button>
      <button @click="toggleAlign">Toggle alignment</button>
      <button @click="toggleTrigger">Toggle trigger</button>
    </div>
    <div v-if="showDatepickers">
      <div class="datepicker-container with-input">
        <h3>Range datepicker with input</h3>
        <div class="datepicker-trigger">
          <input
            type="text"
            id="datepicker-input-trigger"
            :value="formatDates(inputDateOne, inputDateTwo)"
            placeholder="Select dates"
          >

          <airbnb-style-datepicker
            :trigger-element-id="'datepicker-input-trigger'"
            :mode="'range'"
            :date-one="inputDateOne"
            :date-two="inputDateTwo"
            :min-date="'2018-08-28'"
            :months-to-show="2"
            :show-action-buttons="true"
            :show-month-year-select="true"
            @date-one-selected="val => { inputDateOne = val }"
            @date-two-selected="val => { inputDateTwo = val }"
          />
        </div>
      </div>

      <div class="datepicker-container single-with-input">
        <h3>Single datepicker with input</h3>
        <div class="datepicker-trigger">
          <input
            type="text"
            id="datepicker-input-single-trigger"
            :value="formatDates(inputSingleDateOne)"
            placeholder="Select dates"
          >

          <airbnb-style-datepicker
            :trigger-element-id="'datepicker-input-single-trigger'"
            :mode="'single'"
            :date-one="inputSingleDateOne"
            :date-two="inputSingleDateTwo"
            @date-one-selected="val => { inputSingleDateOne = val }"
          />
        </div>
      </div>

      <div class="datepicker-container with-button">
        <h3>Range datepicker with button</h3>
        <div class="datepicker-trigger">
          <button
            id="datepicker-button-trigger"
          >{{ formatDates(buttonDateOne, buttonDateTwo) || 'Select dates' }}</button>

          <airbnb-style-datepicker
            :trigger-element-id="'datepicker-button-trigger'"
            :mode="'range'"
            :date-one="buttonDateOne"
            :date-two="buttonDateTwo"
            :min-date="'2018-04-18'"
            :fullscreen-mobile="true"
            :months-to-show="2"
            :trigger="trigger"
            :offset-y="10"
            :close-after-select="true"
            @date-one-selected="val => { buttonDateOne = val }"
            @date-two-selected="val => { buttonDateTwo = val; trigger = false }"
          />
        </div>
      </div>

      <div class="datepicker-container inline-with-input">
        <h3>Inline datepicker with input</h3>
        <input
          id="datepicker-inline-trigger"
          :value="formatDates(inlineDateOne)"
          type="text"
          placeholder="Select date"
        >
        <airbnb-style-datepicker
          :trigger-element-id="'datepicker-inline-trigger'"
          :mode="'single'"
          :inline="true"
          :fullscreen-mobile="false"
          :date-one="inlineDateOne"
          :months-to-show="2"
          :disabled-dates="['2018-04-30', '2018-05-10', '2018-12-14']"
          :customized-dates="[{ dates: ['2019-03-21', '2019-03-22', '2019-03-23', '2019-03-24'], cssClass: 'booked' }, { dates: ['2019-03-21', '2019-03-22', '2019-03-23', '2019-04-24'], cssClass: 'not-available' }]"
          @date-one-selected="val => { inlineDateOne = val }"
        />
      </div>

      <div class="datepicker-container inline-with-input">
        <span id="datepicker-disabled-dates-trigger"></span>
        <airbnb-style-datepicker
          :trigger-element-id="'datepicker-disabled-dates-trigger'"
          :mode="'single'"
          :inline="true"
          :date-one="withDisabledDatesDateOne"
          :months-to-show="2"
          :disabled-dates="disabledDates"
          :bookings="bookings"
          :calendar-mode="true"
          :custom-style-mode="false"
          @date-one-selected="val => { withDisabledDatesDateOne = val }"
          @show-popup="showPopup"
        />
      </div>
    </div>
  </div>
</template>

<script>
import format from 'date-fns/format'

export default {
  data() {
    return {
      dateFormat: 'YYYY-MM-DD', //'D MMM',
      inputDateOne: '',
      inputDateTwo: '',
      inputSingleDateOne: '',
      inputSingleDateTwo: '',
      buttonDateOne: '',
      buttonDateTwo: '',
      inlineDateOne: '',
      withDisabledDatesDateOne: '',
      callbackDateOne: '',
      callbackDateTwo: '',
      sundayFirst: false,
      alignRight: false,
      showDatepickers: true,
      trigger: false,
      bookings: [],
      timer: {},
    }
  },
  computed: {
    disabledDates() {
      return ['2018-12-30', '2018-12-10', '2018-12-14']
    },
  },
  created() {
    setTimeout(() => {
      this.inputDateOne = '2019-01-12'
      this.inputDateTwo = ''
      this.bookings = [
        {
          startDate: '2020-01-03',
          endDate: '2020-01-10',
          status: 'Complete',
        },
        {
          startDate: '2020-01-11',
          endDate: '2020-01-18',
          status: 'Optional',
        },
        {
          startDate: '2020-01-19',
          endDate: '2020-01-26',
          status: 'Complete',
        },
        {
          startDate: '2020-01-27',
          endDate: '2020-02-03',
          status: 'Optional',
        },
        {
          startDate: '2020-02-04',
          endDate: '2020-02-11',
          status: 'Complete',
        },
        {
          startDate: '2020-02-12',
          endDate: '2020-02-19',
          status: 'Optional',
        },
        {
          startDate: '2020-02-26',
          endDate: '2020-03-03',
          status: 'Complete',
        },
        {
          startDate: '2020-03-04',
          endDate: '2020-03-11',
          status: 'Optional',
        },
      ]
    }, 1000)
  },
  methods: {
    formatDates(dateOne, dateTwo) {
      let formattedDates = ''
      if (dateOne) {
        formattedDates = format(dateOne, this.dateFormat)
      }
      if (dateTwo) {
        formattedDates += ' - ' + format(dateTwo, this.dateFormat)
      }
      return formattedDates
    },
    toggleAlign() {
      this.alignRight = !this.alignRight
    },
    toggleDatepickers() {
      this.showDatepickers = !this.showDatepickers
    },
    toggleTrigger() {
      this.trigger = !this.trigger
    },
    applyMethod() {
      console.log('apply')
    },
    openedMethod() {
      console.log('opened')
    },
    closedMethod() {
      console.log('closed')
      this.trigger = false
    },
    cancelledMethod() {
      console.log('cancelled')
    },
    changeMonthMethod(visibleMonths) {
      console.log('change months', visibleMonths)
    },
    showPopup(val) {
      alert(
        `There is ${val[0].status} booking with booking number ${val[0].bookingNumber} on this date`
      )
    },
  },
}
</script>

<style lang="scss">
html,
body {
  min-height: 200vh;
  font-size: 14px;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu, Cantarell,
    Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  line-height: 18px;
  font-weight: 400;
  -webkit-font-smoothing: antialiased;
  padding: 10px;
}
.app {
  &.align-right {
    text-align: right;
  }
}

h1 {
  font-size: 1.8em;
  line-height: 1.5em;
  text-align: center;
}
.datepicker-container {
  padding: 0 30px 20px;
  border: 1px solid rgba(0, 0, 0, 0.2);
  background: rgba(0, 0, 0, 0.01);
  max-width: 600px;
  margin: 0 auto 30px;
  border-radius: 12px;
}

#datepicker-button-trigger {
  background: #008489;
  border: 1px solid #008489;
  color: white;
  padding: 6px 10px;
  border-radius: 4px;
  font-size: 15px;
  font-weight: bold;
  text-align: center;
  min-width: 200px;
}
input {
  padding: 6px 10px;
  border: 1px solid rgba(0, 0, 0, 0.2);
}
.with-input {
  .datepicker-trigger {
    //padding-right: 40px;
  }
}
.with-button {
  .datepicker-trigger {
    //padding-left: 10px;
  }
}
// .inline-with-input {
//   width: 600px;
//   input {
//     width: 100%;
//   }
// }
.buttons {
  max-width: 500px;
  margin: 0 auto 30px;
  text-align: center;
}
</style>
