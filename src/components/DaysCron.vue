<template>
  <v-card>
    <v-card-title>
      <v-icon large left>
        mdi-calendar-range
      </v-icon>
      <span class="title headline mb-1">Дни</span>
    </v-card-title>
    <v-card-text>
      <v-radio-group @change="resetDayData" v-model="dayOption.key">
        <v-radio label="Каждый день" value="everyDay"></v-radio>
        <v-radio value="everyDayAt">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="everyAnyDay"
                    @input="everyAnyDayFn"
                    prefix="Каждый"
                    suffix="день"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysOfWeekList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="startDay"
                    @input="startDayFn"
                    prefix="начиная с"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysOfWeekIndexList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
        <v-radio value="everyDayOfMonthAt">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="everyAnyDayOfMonth"
                    @input="everyAnyDayOfMonthFn"
                    prefix="Каждый"
                    suffix="день"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysIndexOfMonthList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="startDayOfMonth"
                    @input="startDayOfMonthFn"
                    prefix="начиная с"
                    suffix="дня месяца"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysIndexOfMonthList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
        <v-radio value="daysSpecific">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="daysSpecific"
                    @change="daysSpecificFn"
                    prefix="По указанным"
                    suffix="дням недели"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysOfWeekIndexList"
                    single-line
                    chips
                    multiple
                    deletable-chips
                  /> </v-col
              ></v-row></div
          ></template>
        </v-radio>
        <v-radio
          label="Día especifico del mes uno o varios"
          value="daysOfMonthSpecific"
        >
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="daysOfMonthSpecific"
                    @change="daysOfMonthSpecificFn"
                    prefix="По указанным"
                    suffix="дням месяца"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysIndexOfMonthList"
                    single-line
                    chips
                    multiple
                    deletable-chips
                  /> </v-col
              ></v-row></div
          ></template>
        </v-radio>
        <v-radio value="everyDayBetween">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    @input="betweenDayFn"
                    v-model="betweenDay"
                    prefix="Каждый день с"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysOfWeekIndexList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    @input="andDayFn"
                    v-model="andDay"
                    prefix="по"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysOfWeekIndexList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
        <v-radio label="Последний день месяца" value="lastDayOfMonth" />
        <v-radio
          label="Последний будний день недели месяца"
          value="lastWeekdayOfMonth"
        />
        <v-radio value="lastDaySpecificOfWeekOfMonth">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="lastDaySpecificOfWeekOfMonth"
                    @input="lastDaySpecificOfWeekOfMonthFn"
                    prefix="Последний"
                    suffix="месяца"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysOfWeekIndexList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
        <v-radio value="beforeEndOfMonth">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="beforeEndOfMonth"
                    @input="beforeEndOfMonthFn"
                    prefix="За"
                    suffix="дня до конца месяца"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysIndexOfMonthList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
        <v-radio value="nearestWeekdayOfMonth">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="nearestWeekdayOfMonth"
                    @input="nearestWeekdayOfMonthFn"
                    prefix="Ближайший будний день"
                    suffix="числа месяца"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysIndexOfMonthList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
        <v-radio value="weekdaysPerMonth">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="weekdays"
                    @input="weekdaysFn"
                    prefix="В"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="weekdaysPerMonthList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="daysOfMonth"
                    @input="daysOfMonthFn"
                    suffix="месяца"
                    item-text="dayLabel"
                    item-value="dayValue"
                    :items="daysOfWeekIndexList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
      </v-radio-group>
    </v-card-text>
  </v-card>
</template>
<script>
export default {
  name: "DaysCron",
  props: {
    value: {
      type: Object
    }
  },
  data: () => ({
    dayOption: {
      key: null,
      values: {
        dayOfMonth: "?",
        dayOfWeek: "*"
      }
    },
    everyAnyDay: null,
    startDay: null,
    daysSpecific: [],
    daysOfMonthSpecific: [],
    betweenDay: null,
    andDay: null,
    initDay: 0,
    colCount: 10,
    rowCount: 4,
    daysOfWeekIndexList: [],
    daysOfWeekList: [],
    daysOfMonthList: [],
    daysIndexOfMonthList: [],
    weekdaysPerMonthList: [],
    everyAnyDayOfMonth: null,
    startDayOfMonth: null,
    lastDaySpecificOfWeekOfMonth: null,
    beforeEndOfMonth: null,
    nearestWeekdayOfMonth: null,
    weekdays: null,
    daysOfMonth: null,
    reseting: false
  }),
  watch: {
    value: {
      deep: true,
      handler() {
        if (
          JSON.stringify(this.value) !== JSON.stringify(this.dayOption.values)
        ) {
          this.resetValues();
          this.dayOption.values = this.value;
          this.setValues();
        }
      }
    },
    dayOption: {
      deep: true,
      handler(oldValue, newValue) {
        if (!this.reseting) {
          if (newValue.key === "everyDay") {
            this.dayOption.values.dayOfWeek = "*";
            this.resetDayData();
          }
          if (newValue.key === "everyDayAt") {
            if (!this.startDay || this.startDay === "") {
              this.startDay = 1;
            }
            if (!this.everyAnyDay || this.everyAnyDay === "") {
              this.everyAnyDay = 1;
            }
            this.resetDayOfMonth();
            this.dayOption.values.dayOfWeek =
              parseInt(this.startDay) + "/" + parseInt(this.everyAnyDay);
          }
          if (newValue.key === "everyDayOfMonthAt") {
            if (!this.startDayOfMonth || this.startDayOfMonth === "") {
              this.startDayOfMonth = 1;
            }
            if (!this.everyAnyDayOfMonth || this.everyAnyDayOfMonth === "") {
              this.everyAnyDayOfMonth = 1;
            }
            this.resetDayOfWeek();
            this.dayOption.values.dayOfMonth =
              parseInt(this.startDayOfMonth) +
              "/" +
              parseInt(this.everyAnyDayOfMonth);
          }
          if (newValue.key === "daysSpecific") {
            let valuesInt = [];
            if (this.daysSpecific.length == 0) {
              this.daysSpecific.push(1);
            }
            this.daysSpecific.forEach((itemValue) => {
              valuesInt.push(parseInt(itemValue));
            });
            this.dayOption.values.dayOfWeek = valuesInt.toString();
            this.resetDayOfMonth();
          }
          if (newValue.key === "daysOfMonthSpecific") {
            let valuesInt = [];
            if (this.daysOfMonthSpecific.length == 0) {
              this.daysOfMonthSpecific.push(1);
            }
            this.daysOfMonthSpecific.forEach((itemValue) => {
              valuesInt.push(parseInt(itemValue));
            });
            this.dayOption.values.dayOfMonth = valuesInt.toString();
            this.resetDayOfWeek();
          }
          if (newValue.key === "everyDayBetween") {
            if (!this.betweenDay || this.betweenDay === "") {
              this.betweenDay = 1;
            }
            if (!this.andDay || this.andDay === "") {
              this.andDay = 1;
            }
            this.dayOption.values.dayOfWeek =
              parseInt(this.betweenDay) + "-" + parseInt(this.andDay);
          }
          if (newValue.key === "lastDayOfMonth") {
            this.resetDayOfWeek();
            this.dayOption.values.dayOfMonth = "L";
          }
          if (newValue.key === "lastWeekdayOfMonth") {
            this.resetDayOfWeek();
            this.dayOption.values.dayOfMonth = "LW";
          }
          if (newValue.key === "lastDaySpecificOfWeekOfMonth") {
            this.resetDayOfMonth();
            if (
              !this.lastDaySpecificOfWeekOfMonth ||
              this.lastDaySpecificOfWeekOfMonth === ""
            ) {
              this.lastDaySpecificOfWeekOfMonth = 1;
            }
            this.dayOption.values.dayOfWeek =
              this.lastDaySpecificOfWeekOfMonth + "L";
          }
          if (newValue.key === "beforeEndOfMonth") {
            this.resetDayOfWeek();
            if (!this.beforeEndOfMonth || this.beforeEndOfMonth === "") {
              this.beforeEndOfMonth = 1;
            }
            this.dayOption.values.dayOfMonth = "L-" + this.beforeEndOfMonth;
          }
          if (newValue.key === "nearestWeekdayOfMonth") {
            this.resetDayOfWeek();
            if (
              !this.nearestWeekdayOfMonth ||
              this.nearestWeekdayOfMonth === ""
            ) {
              this.nearestWeekdayOfMonth = 1;
            }
            this.dayOption.values.dayOfMonth = this.nearestWeekdayOfMonth + "W";
          }
          if (newValue.key === "weekdaysPerMonth") {
            this.resetDayOfMonth();
            if (!this.weekdays || this.weekdays === "") {
              this.weekdays = 1;
            }
            if (!this.daysOfMonth || this.daysOfMonth === "") {
              this.daysOfMonth = 1;
            }
            this.dayOption.values.dayOfWeek =
              parseInt(this.daysOfMonth) + "#" + parseInt(this.weekdays);
          }
          this.updateValue();
        }
      }
    }
  },
  methods: {
    resetValues() {
      this.reseting = true;
      this.dayOption.key = null;
      this.dayOption.values.dayOfMonth = "?";
      this.dayOption.values.dayOfWeek = "*";
      this.everyAnyDay = null;
      this.startDay = null;
      this.daysSpecific = [];
      this.daysOfMonthSpecific = [];
      this.betweenDay = null;
      this.andDay = null;
      this.everyAnyDayOfMonth = null;
      this.startDayOfMonth = null;
      this.lastDaySpecificOfWeekOfMonth = null;
      this.beforeEndOfMonth = null;
      this.nearestWeekdayOfMonth = null;
      this.weekdays = null;
      this.daysOfMonth = null;
      this.reseting = false;
    },
    updateValue() {
      this.$emit("input", JSON.parse(JSON.stringify(this.dayOption.values)));
    },
    resetDayOfMonth() {
      this.dayOption.values.dayOfMonth = "?";
    },
    resetDayOfWeek() {
      this.dayOption.values.dayOfWeek = "?";
    },
    resetDayData() {
      this.dayOption.values.dayOfMonth = "?";
      this.dayOption.values.dayOfWeek = "*";
    },
    everyAnyDayFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.startDay || this.startDay === "") {
        this.startDay = 1;
      }
      this.dayOption.key = "everyDayAt";
      this.dayOption.values.dayOfWeek =
        parseInt(this.startDay) + "/" + parseInt(e);
    },
    startDayFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.everyAnyDay || this.everyAnyDay === "") {
        this.everyAnyDay = 1;
      }
      this.dayOption.key = "everyDayAt";
      this.dayOption.values.dayOfWeek =
        parseInt(e) + "/" + parseInt(this.everyAnyDay);
    },
    everyAnyDayOfMonthFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.startDayOfMonth || this.startDayOfMonth === "") {
        this.startDayOfMonth = 1;
      }
      this.dayOption.key = "everyDayOfMonthAt";
      this.dayOption.values.dayOfMonth =
        parseInt(this.startDayOfMonth) + "/" + parseInt(e);
    },
    startDayOfMonthFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.everyAnyDayOfMonth || this.everyAnyDayOfMonth === "") {
        this.everyAnyDayOfMonth = 1;
      }
      this.dayOption.key = "everyDayOfMonthAt";
      this.dayOption.values.dayOfMonth =
        parseInt(e) + "/" + parseInt(this.everyAnyDayOfMonth);
    },
    betweenDayFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.andDay || this.andDay === "") {
        this.andDay = 1;
      }
      this.dayOption.values.dayOfWeek =
        parseInt(e) + "-" + parseInt(this.andDay);
    },
    andDayFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.betweenDay || this.betweenDay === "") {
        this.betweenDay = 1;
      }
      this.dayOption.values.dayOfWeek =
        parseInt(this.betweenDay) + "-" + parseInt(e);
    },
    lastDaySpecificOfWeekOfMonthFn(e) {
      if (!e || e === "") {
        e = 1;
        this.lastDaySpecificOfWeekOfMonth = 1;
      }
      this.dayOption.key = "lastDaySpecificOfWeekOfMonth";
      this.dayOption.values.dayOfWeek = parseInt(e) + "L";
    },
    beforeEndOfMonthFn(e) {
      if (!e || e === "") {
        e = 1;
        this.beforeEndOfMonth = 1;
      }
      this.dayOption.key = "beforeEndOfMonth";
      this.dayOption.values.dayOfMonth = "L-" + parseInt(e);
    },
    nearestWeekdayOfMonthFn(e) {
      if (!e || e === "") {
        e = 1;
        this.nearestWeekdayOfMonth = 1;
      }
      this.dayOption.key = "nearestWeekdayOfMonth";
      this.dayOption.values.dayOfMonth = parseInt(e) + "W";
    },
    weekdaysFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.daysOfMonth || this.daysOfMonth === "") {
        this.daysOfMonth = 1;
      }
      this.dayOption.key = "weekdaysPerMonth";
      this.dayOption.values.dayOfWeek =
        parseInt(this.daysOfMonth) + "#" + parseInt(e);
    },
    daysOfMonthFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.weekdays || this.weekdays === "") {
        this.weekdays = 1;
      }
      this.dayOption.key = "weekdaysPerMonth";
      this.dayOption.values.dayOfWeek =
        parseInt(e) + "#" + parseInt(this.weekdays);
    },
    daysSpecificFn(value) {
      this.dayOption.key = "daysSpecific";
      let valuesInt = [];
      value.forEach((itemValue) => {
        valuesInt.push(parseInt(itemValue));
      });
      this.dayOption.values.dayOfWeek = valuesInt.toString();
      this.resetDayOfMonth();
    },
    daysOfMonthSpecificFn(value) {
      this.dayOption.key = "daysOfMonthSpecific";
      let valuesInt = [];
      value.forEach((itemValue) => {
        valuesInt.push(parseInt(itemValue));
      });
      this.dayOption.values.dayOfMonth = valuesInt.toString();
      this.resetDayOfWeek();
    },
    buildDays() {
      for (let d = 0; d < 7; d++) {
        let indexDayLabel = "" + d;
        if (d == 0) {
          indexDayLabel = "Воскресенье";
        } else if (d == 1) {
          indexDayLabel = "Понедельник";
        } else if (d == 2) {
          indexDayLabel = "Вторник";
        } else if (d == 3) {
          indexDayLabel = "Среда";
        } else if (d == 4) {
          indexDayLabel = "Четверг";
        } else if (d == 5) {
          indexDayLabel = "Пятница";
        } else if (d == 6) {
          indexDayLabel = "Суббота";
        }
        let itemDay = {
          dayLabel: indexDayLabel,
          dayValue: d + 1
        };
        this.daysOfWeekIndexList.push(itemDay);
        let itemDayWeek = {
          dayLabel: "0" + (d + 1),
          dayValue: d + 1
        };
        this.daysOfWeekList.push(itemDayWeek);
      }
    },
    buildDaysOfMonth() {
      let indexDay = parseInt("" + this.initDay);
      let everyAnyDay = 1;
      for (let r = 0; r < this.rowCount; r++) {
        let daysByRow = [];
        for (let c = 0; c < this.colCount; c++) {
          let everyDayLabel = "" + everyAnyDay;
          if (everyAnyDay < 10) {
            everyDayLabel = "0" + everyDayLabel;
          }
          let itemDay = {
            dayLabel: everyDayLabel,
            dayValue: indexDay + 1
          };
          if (indexDay < 31) {
            daysByRow.push(itemDay);
            this.daysIndexOfMonthList.push(itemDay);
          }
          indexDay++;
          everyAnyDay++;
        }
        this.daysOfMonthList.push(daysByRow);
      }
    },
    buildWeekdaysPerMonth() {
      for (let i = 0; i < 5; i++) {
        let itemDay = {
          dayLabel: "0" + (i + 1),
          dayValue: i + 1
        };
        this.weekdaysPerMonthList.push(itemDay);
      }
    },
    setValues() {
      if (this.value.dayOfWeek.includes("*")) {
        this.dayOption.key = "everyDay";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };
      }

      if (this.value.dayOfWeek.includes("/")) {
        this.dayOption.key = "everyDayAt";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };

        let inputValue = this.value.dayOfWeek.split("/");
        this.everyAnyDay = parseInt(inputValue[1]);
        this.startDay = parseInt(inputValue[0]);
      }

      if (this.value.dayOfMonth.includes("/")) {
        this.dayOption.key = "everyDayOfMonthAt";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };

        let inputValue = this.value.dayOfMonth.split("/");
        this.everyAnyDayOfMonth = parseInt(inputValue[1]);
        this.startDayOfMonth = parseInt(inputValue[0]);
      }

      if (
        this.value.dayOfWeek.includes(",") ||
        (parseInt(this.value.dayOfWeek) >= 0 && !isNaN(this.value.dayOfWeek))
      ) {
        this.dayOption.key = "daysSpecific";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };

        let inputValue = JSON.parse("[" + this.value.dayOfWeek + "]");
        this.daysSpecific = inputValue;
      }

      if (
        this.value.dayOfMonth.includes(",") ||
        (parseInt(this.value.dayOfMonth) >= 0 && !isNaN(this.value.dayOfMonth))
      ) {
        this.dayOption.key = "daysOfMonthSpecific";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };

        let inputValue = JSON.parse("[" + this.value.dayOfMonth + "]");
        this.daysOfMonthSpecific = inputValue;
      }

      if (this.value.dayOfWeek.includes("-")) {
        this.dayOption.key = "everyDayBetween";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };

        let inputValue = this.value.dayOfWeek.split("-");
        this.betweenDay = parseInt(inputValue[0]);
        this.andDay = parseInt(inputValue[1]);
      }

      if (this.value.dayOfMonth === "L") {
        this.dayOption.key = "lastDayOfMonth";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };
      }

      if (this.value.dayOfMonth === "LW") {
        this.dayOption.key = "lastWeekdayOfMonth";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };
      }

      if (this.value.dayOfWeek.endsWith("L")) {
        this.dayOption.key = "lastDaySpecificOfWeekOfMonth";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };
        this.lastDaySpecificOfWeekOfMonth = parseInt(
          this.value.dayOfWeek.replace("L", "")
        );
      }

      if (this.value.dayOfMonth.startsWith("L-")) {
        this.dayOption.key = "beforeEndOfMonth";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };
        this.beforeEndOfMonth = parseInt(
          this.value.dayOfMonth.replace("L-", "")
        );
      }

      if (this.value.dayOfMonth.endsWith("W")) {
        this.dayOption.key = "nearestWeekdayOfMonth";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };
        this.nearestWeekdayOfMonth = parseInt(
          this.value.dayOfMonth.replace("W", "")
        );
      }

      if (this.value.dayOfWeek.includes("#")) {
        this.dayOption.key = "weekdaysPerMonth";
        this.dayOption.values = {
          dayOfMonth: this.value.dayOfMonth,
          dayOfWeek: this.value.dayOfWeek
        };
        let inputValue = this.value.dayOfWeek.split("#");
        this.weekdays = parseInt(inputValue[0]);
        this.daysOfMonth = parseInt(inputValue[1]);
      }
    }
  },
  created() {
    this.setValues();
    this.buildDays();
    this.buildDaysOfMonth();
    this.buildWeekdaysPerMonth();
  }
};
</script>
