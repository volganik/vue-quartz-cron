<template>
  <v-card>
    <v-card-title>
      <v-icon large left>
        mdi-clock-outline
      </v-icon>
      <span class="title headline mb-1">Минуты</span>
    </v-card-title>
    <v-card-text>
      <v-radio-group @change="resetMinuteData" v-model="minuteOption.key">
        <v-radio label="Каждую минуту" value="everyMinute"></v-radio>
        <v-radio value="everyMinuteAt">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="everyAnyMinute"
                    @input="everyAnyMinuteFn"
                    prefix="Кажудую"
                    suffix="минуту"
                    item-text="minuteLabel"
                    item-value="minuteValue"
                    :items="minutesList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="startMinute"
                    @input="startMinuteFn"
                    prefix="начиная с минуты"
                    item-text="minuteLabel"
                    item-value="minuteValue"
                    :items="minutesIndexList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
        <v-radio value="minutesSpecific">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="minutesSpecific"
                    @change="minutesSpecificFn"
                    prefix="В указанные"
                    suffix="минуты"
                    item-text="minuteLabel"
                    item-value="minuteValue"
                    :items="minutesIndexList"
                    single-line
                    chips
                    multiple
                    deletable-chips
                  /> </v-col
              ></v-row></div
          ></template>
        </v-radio>
        <v-radio value="everyMinuteBetween">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    @input="betweenMinuteFn"
                    v-model="betweenMinute"
                    prefix="Каждую минуту с"
                    suffix="минуты"
                    item-text="minuteLabel"
                    item-value="minuteValue"
                    :items="minutesIndexList"
                    item-disabled="disabledBetweenMinuteItem"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    @input="andMinuteFn"
                    v-model="andMinute"
                    prefix="по"
                    suffix="минуту"
                    item-text="minuteLabel"
                    item-value="minuteValue"
                    :items="minutesIndexList"
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
  name: "MinutesCron",
  props: {
    value: { type: String, default: "0" }
  },
  data: () => ({
    minuteOption: {
      key: null,
      value: 0
    },
    everyAnyMinute: null,
    startMinute: null,
    minutesSpecific: [],
    betweenMinute: null,
    andMinute: null,
    minutesIndexList: [],
    minutesList: [],
    reseting: false
  }),
  watch: {
    value() {
      if (this.value !== this.minuteOption.value) {
        this.resetValues();
        this.minuteOption.value = this.value;
        this.setValues();
      }
    },
    minuteOption: {
      deep: true,
      handler(oldValue, newValue) {
        if (!this.reseting) {
          if (newValue.key === "everyMinute") {
            this.minuteOption.value = "*";
          }
          if (newValue.key === "everyMinuteAt") {
            if (!this.startMinute || this.startMinute === "") {
              this.startMinute = 0;
            }
            if (!this.everyAnyMinute || this.everyAnyMinute === "") {
              this.everyAnyMinute = 1;
            }
            this.minuteOption.value =
              parseInt(this.startMinute) + "/" + parseInt(this.everyAnyMinute);
          }
          if (newValue.key === "minutesSpecific") {
            let valuesInt = [];
            if (this.minutesSpecific.length == 0) {
              this.minutesSpecific.push(0);
            }
            this.minutesSpecific.forEach((itemValue) => {
              valuesInt.push(parseInt(itemValue));
            });
            this.minuteOption.value = valuesInt.toString();
          }
          if (newValue.key === "everyMinuteBetween") {
            if (!this.betweenMinute || this.betweenMinute === "") {
              this.betweenMinute = 0;
            }
            if (!this.andMinute || this.andMinute === "") {
              this.andMinute = 0;
            }
            this.minuteOption.value =
              parseInt(this.betweenMinute) + "-" + parseInt(this.andMinute);
          }
          this.updateValue();
        }
      }
    },
    andMinute(value) {
      this.minutesIndexList = this.minutesIndexList.map((item) => {
        item.disabledBetweenMinuteItem = item.minuteValue > value;
        return item;
      });
    }
  },
  methods: {
    resetValues() {
      this.reseting = true;
      this.minuteOption.key = null;
      this.minuteOption.value = 0;
      this.everyAnyMinute = null;
      this.startMinute = null;
      this.minutesSpecific = [];
      this.betweenMinute = null;
      this.andMinute = null;
      this.reseting = false;
    },
    updateValue() {
      this.$emit("input", this.minuteOption.value);
    },
    resetMinuteData() {
      this.minuteOption.value = null;
    },
    everyAnyMinuteFn(e) {
      if (!e || e === "") {
        e = 1;
      }
      if (!this.startMinute || this.startMinute === "") {
        this.startMinute = 0;
      }
      this.minuteOption.value = parseInt(this.startMinute) + "/" + parseInt(e);
    },
    startMinuteFn(e) {
      if (!e || e === "") {
        e = 0;
      }
      if (!this.everyAnyMinute || this.everyAnyMinute === "") {
        this.everyAnyMinute = 1;
      }
      this.minuteOption.value =
        parseInt(e) + "/" + parseInt(this.everyAnyMinute);
    },
    betweenMinuteFn(e) {
      if (!e || e === "") {
        e = 0;
      }
      if (!this.andMinute || this.andMinute === "") {
        this.andMinute = 0;
      }
      this.minuteOption.value = parseInt(e) + "-" + parseInt(this.andMinute);
    },
    andMinuteFn(e) {
      if (!e || e === "") {
        e = 0;
      }
      if (!this.betweenMinute || this.betweenMinute === "") {
        this.betweenMinute = 0;
      }
      this.minuteOption.value =
        parseInt(this.betweenMinute) + "-" + parseInt(e);
    },
    minutesSpecificFn(value) {
      this.minuteOption.key = "minutesSpecific";
      let valuesInt = [];
      value.forEach((itemValue) => {
        valuesInt.push(parseInt(itemValue));
      });
      this.minuteOption.value = valuesInt.toString();
    },
    buildMinutes() {
      for (let m = 0; m < 60; m++) {
        let indexMinuteLabel = "" + m;
        if (m < 10) {
          indexMinuteLabel = "0" + indexMinuteLabel;
        }
        let itemMinute = {
          minuteLabel: indexMinuteLabel,
          minuteValue: m
        };
        this.minutesIndexList.push(itemMinute);

        let minuteLabel = "" + (m + 1);
        if (m < 10) {
          minuteLabel = "0" + minuteLabel;
        }
        itemMinute = {
          minuteLabel: minuteLabel,
          minuteValue: m + 1
        };

        this.minutesList.push(itemMinute);
      }
    },
    setValues() {
      if (this.value.includes("*")) {
        this.minuteOption.key = "everyMinute";
        this.minuteOption.value = this.value;
      }

      if (this.value.includes("/")) {
        this.minuteOption.key = "everyMinuteAt";
        this.minuteOption.value = this.value;
        let inputValue = this.value.split("/");
        this.everyAnyMinute = parseInt(inputValue[1]);
        this.startMinute = parseInt(inputValue[0]);
      }

      if (
        this.value.includes(",") ||
        (parseInt(this.value) >= 0 && !isNaN(this.value))
      ) {
        this.minuteOption.key = "minutesSpecific";
        this.minuteOption.value = this.value;
        let inputValue = JSON.parse("[" + this.value + "]");
        this.minutesSpecific = inputValue;
      }

      if (this.value.includes("-")) {
        this.minuteOption.key = "everyMinuteBetween";
        this.minuteOption.value = this.value;
        let inputValue = this.value.split("-");
        this.betweenMinute = parseInt(inputValue[0]);
        this.andMinute = parseInt(inputValue[1]);
      }
    }
  },
  created() {
    this.setValues();
    this.buildMinutes();
  }
};
</script>
