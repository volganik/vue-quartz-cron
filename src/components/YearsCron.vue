<template>
  <v-card>
    <v-card-title>
      <v-icon large left>
        mdi-calendar-week
      </v-icon>
      <span class="title headline mb-1">Годы</span>
    </v-card-title>
    <v-card-text>
      <v-radio-group @change="resetYearData" v-model="yearOption.key">
        <v-radio label="Каждый год" value="everyYear"></v-radio>
        <v-radio value="everyYearAt">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="everyAnyYear"
                    @input="everyAnyYearFn"
                    prefix="Каждый"
                    suffix="год"
                    item-text="yearLabel"
                    item-value="yearValue"
                    :items="yearsList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="startYear"
                    @input="startYearFn"
                    prefix="начиная с"
                    item-text="yearLabel"
                    item-value="yearValue"
                    :items="yearsIndexList"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
              </v-row>
            </div>
          </template>
        </v-radio>
        <v-radio value="yearsSpecific">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    v-model="yearsSpecific"
                    @change="yearsSpecificFn"
                    prefix="Указанные"
                    suffix="года"
                    item-text="yearLabel"
                    item-value="yearValue"
                    :items="yearsIndexList"
                    single-line
                    chips
                    multiple
                    deletable-chips
                  /> </v-col
              ></v-row></div
          ></template>
        </v-radio>
        <v-radio value="everyYearBetween">
          <template v-slot:label>
            <div>
              <v-row>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    @input="betweenYearFn"
                    v-model="betweenYear"
                    prefix="Каждый год между"
                    suffix=""
                    item-text="yearLabel"
                    item-value="yearValue"
                    :items="yearsIndexList"
                    item-disabled="disabledBetweenYearItem"
                    single-line
                    chips
                    deletable-chips
                  />
                </v-col>
                <v-col class="pt-0 pb-0">
                  <v-select
                    class="pt-0 pb-0 mt-0"
                    @input="andYearFn"
                    v-model="andYear"
                    prefix="и"
                    suffix="годом"
                    item-text="yearLabel"
                    item-value="yearValue"
                    :items="yearsIndexList"
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
  name: "YearsCron",
  props: {
    value: { type: String, default: "0" }
  },
  data: () => ({
    yearOption: {
      key: null,
      value: 0
    },
    everyAnyYear: null,
    startYear: null,
    yearsSpecific: [],
    betweenYear: null,
    andYear: null,
    initYear: 2016,
    yearsIndexList: [],
    yearsList: [],
    reseting: false
  }),
  watch: {
    value() {
      if (this.value !== this.yearOption.value) {
        this.resetValues();
        this.yearOption.value = this.value;
        this.setValues();
      }
    },
    yearOption: {
      deep: true,
      handler(oldValue, newValue) {
        if (!this.reseting) {
          if (newValue.key === "everyYear") {
            this.yearOption.value = "*";
          }
          if (newValue.key === "everyYearAt") {
            if (!this.startYear || this.startYear === "") {
              this.startYear = this.initYear;
            }
            if (!this.everyAnyYear || this.everyAnyYear === "") {
              this.everyAnyYear = 1;
            }
            this.yearOption.value =
              parseInt(this.startYear) + "/" + parseInt(this.everyAnyYear);
          }
          if (newValue.key === "yearsSpecific") {
            let valuesInt = [];
            if (this.yearsSpecific.length == 0) {
              this.yearsSpecific.push(this.initYear);
            }
            this.yearsSpecific.forEach((itemValue) => {
              valuesInt.push(parseInt(itemValue));
            });
            this.yearOption.value = valuesInt.toString();
          }
          if (newValue.key === "everyYearBetween") {
            if (!this.betweenYear || this.betweenYear === "") {
              this.betweenYear = this.initYear;
            }
            if (!this.andYear || this.andYear === "") {
              this.andYear = this.initYear;
            }
            this.yearOption.value =
              parseInt(this.betweenYear) + "-" + parseInt(this.andYear);
          }
          this.updateValue();
        }
      }
    },
    andYear(value) {
      this.yearsIndexList = this.yearsIndexList.map((item) => {
        item.disabledBetweenYearItem = item.yearValue > value;
        return item;
      });
    }
  },
  methods: {
    resetValues() {
      this.reseting = true;
      this.yearOption.key = null;
      this.yearOption.value = 0;
      this.everyAnyYear = null;
      this.startYear = null;
      this.yearsSpecific = [];
      this.betweenYear = null;
      this.andYear = null;
      this.reseting = false;
    },
    updateValue() {
      this.$emit("input", this.yearOption.value + "@");
    },
    resetYearData() {
      this.yearOption.value = null;
    },
    everyAnyYearFn(e) {
      if (!e || e === "") {
        e = this.initYear;
      }
      if (!this.startYear || this.startYear === "") {
        this.startYear = this.initYear;
      }
      this.yearOption.value = parseInt(this.startYear) + "/" + parseInt(e);
    },
    startYearFn(e) {
      if (!e || e === "") {
        e = this.initYear;
      }
      if (!this.everyAnyYear || this.everyAnyYear === "") {
        this.everyAnyYear = 1;
      }
      this.yearOption.value = parseInt(e) + "/" + parseInt(this.everyAnyYear);
    },
    betweenYearFn(e) {
      if (!e || e === "") {
        e = this.initYear;
      }
      if (!this.andYear || this.andYear === "") {
        this.andYear = this.initYear;
      }
      this.yearOption.value = parseInt(e) + "-" + parseInt(this.andYear);
    },
    andYearFn(e) {
      if (!e || e === "") {
        e = this.initYear;
      }
      if (!this.betweenYear || this.betweenYear === "") {
        this.betweenYear = this.initYear;
      }
      this.yearOption.value = parseInt(this.betweenYear) + "-" + parseInt(e);
    },
    yearsSpecificFn(value) {
      this.yearOption.key = "yearsSpecific";
      let valuesInt = [];
      value.forEach((itemValue) => {
        valuesInt.push(parseInt(itemValue));
      });
      this.yearOption.value = valuesInt.toString();
    },
    buildYears() {
      let indexYear = 0;
      for (let y = this.initYear; y < 2100; y++) {
        let everyYearLabel = "" + (indexYear + 1);
        if (y < 10) {
          everyYearLabel = "0" + everyYearLabel;
        }
        let itemYear = {
          yearLabel: "" + y,
          yearValue: y
        };
        let itemIndexYear = {
          yearLabel: everyYearLabel,
          yearValue: indexYear + 1
        };
        this.yearsIndexList.push(itemYear);
        this.yearsList.push(itemIndexYear);
        indexYear++;
      }
    },
    setValues() {
      if (this.value.includes("*")) {
        this.yearOption.key = "everyYear";
        this.yearOption.value = this.value;
      }

      if (this.value.includes("/")) {
        this.yearOption.key = "everyYearAt";
        this.yearOption.value = this.value;
        let inputValue = this.value.split("/");
        this.everyAnyYear = parseInt(inputValue[1]);
        this.startYear = parseInt(inputValue[0]);
      }

      if (
        this.value.includes(",") ||
        (parseInt(this.value) >= 0 && !isNaN(this.value))
      ) {
        this.yearOption.key = "yearsSpecific";
        this.yearOption.value = this.value;
        let inputValue = JSON.parse("[" + this.value + "]");
        this.yearsSpecific = inputValue;
      }

      if (this.value.includes("-")) {
        this.yearOption.key = "everyYearBetween";
        this.yearOption.value = this.value;
        let inputValue = this.value.split("-");
        this.betweenYear = parseInt(inputValue[0]);
        this.andYear = parseInt(inputValue[1]);
      }
    }
  },
  created() {
    this.setValues();
    this.buildYears();
  }
};
</script>
