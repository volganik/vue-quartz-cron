<template>
  <v-card tile height="auto">
    <v-card-title v-if="showDescription" class="primary white--text">
      <span class="title headline mb-1">{{ descriptionValue }}</span>
    </v-card-title>
    <v-card-subtitle v-if="showValue" class="primary white--text">
        <span class="title headline mb-1 text-decoration-underline">{{ value  }}</span>
    </v-card-subtitle>

    <v-card-text class="cardText">

  <v-tabs
      v-model="tab"
      background-color="deep-purple accent-4"
      centered
      dark
      icons-and-text
    >
 
   <v-tabs-slider></v-tabs-slider>

      <v-tab href="#tab-1">Секунды</v-tab>
      <v-tab href="#tab-2">Минуты</v-tab>
      <v-tab href="#tab-3">Часы</v-tab>
      <v-tab href="#tab-4">Дни</v-tab>
      <v-tab href="#tab-5">Месяцы</v-tab>
      <v-tab href="#tab-6">Годы</v-tab>

  </v-tabs>

 <v-tabs-items v-model="tab">
      <v-tab-item value="tab-1">
        <v-card flat>
        <v-col cols="12" sm="6" md="6" lg="6" xl="6">
          <SecondsCron v-model="secondsCron" />
        </v-col>
        </v-card>
      </v-tab-item>
    

      <v-tab-item value="tab-2">
        <v-card flat>
        <v-col cols="12" sm="6" md="6" lg="6" xl="6">
          <MinutesCron v-model="minutesCron" />
        </v-col>
        </v-card>
      </v-tab-item>


      <v-tab-item value="tab-3">
        <v-card flat>
        <v-col cols="12" sm="12" md="12" lg="12" xl="12">
          <HoursCron v-model="hoursCron" />
        </v-col>
                </v-card>
      </v-tab-item>


      <v-tab-item value="tab-4">
        <v-card flat>
        <v-col cols="12" sm="12" md="12" lg="12" xl="12">
          <DaysCron v-model="daysCron" />
        </v-col>
                </v-card>
      </v-tab-item>


      <v-tab-item value="tab-5">
        <v-card flat>
        <v-col cols="12" sm="6" md="6" lg="6" xl="6">
          <MonthsCron v-model="monthsCron" />
        </v-col>
                </v-card>
      </v-tab-item>


      <v-tab-item value="tab-6">
        <v-card flat>
        <v-col cols="12" sm="6" md="6" lg="6" xl="6">
          <YearsCron v-model="yearsCron" />
        </v-col>
                </v-card>
      </v-tab-item>


</v-tabs-items>

    </v-card-text>
  </v-card>
</template>
<style scoped>
.cardText {
  overflow: auto;
  max-height: 600px;
  min-height: 600px;
}
</style>
<script>
import SecondsCron from "@/components/SecondsCron.vue";
import MinutesCron from "@/components/MinutesCron.vue";
import HoursCron from "@/components/HoursCron.vue";
import DaysCron from "@/components/DaysCron.vue";
import MonthsCron from "@/components/MonthsCron.vue";
import YearsCron from "@/components/YearsCron.vue";
import cronstrue from "cronstrue/i18n";
import cron from "cron-validate";
const presetCronValidate = {
  override: {
    useYears: true,
    useSeconds: true,
    allowOnlyOneBlankDayField: true,
    mustHaveBlankDayField: false,
    useBlankDay: true,
    useLastDayOfMonth: true,
    useLastDayOfWeek: true,
    useNearestWeekday: true,
    useNthWeekdayOfMonth: true
  }
};
export default {
  name: "CronQuartz",
  props: {
    value: { type: String },
    showDescription: { default: false, type: Boolean },
    showValue: { default: false, type: Boolean }
  },
  components: {
    SecondsCron,
    MinutesCron,
    HoursCron,
    DaysCron,
    MonthsCron,
    YearsCron
  },
  data: () => ({
    tab: null,
    descriptionValue: null,
    cronExpression: null,
    secondsCron: "*",
    minutesCron: "*",
    hoursCron: "*",
    daysCron: {
      dayOfWeek: "*",
      dayOfMonth: "?"
    },
    monthsCron: "*",
    yearsCron: "*"
  }),
  watch: {
    value() {
      if (this.cronExpression && this.value !== this.cronExpression) {
        const cronResult = cron(this.value, presetCronValidate);
        if (cronResult.isValid()) {
          this.secondsCron = cronResult.value.seconds;
          this.minutesCron = cronResult.value.minutes;
          this.hoursCron = cronResult.value.hours;
          this.daysCron.dayOfWeek = cronResult.value.daysOfWeek;
          this.daysCron.dayOfMonth = cronResult.value.daysOfMonth;
          this.monthsCron = cronResult.value.months;
          this.yearsCron = cronResult.value.years;
        }
      }
      this.__updateValue();
    },
    secondsCron() {
      this.__updateValue();
    },
    minutesCron() {
      this.__updateValue();
    },
    hoursCron() {
      this.__updateValue();
    },
    daysCron: {
      deep: true,
      handler() {
        this.__updateValue();
      }
    },
    monthsCron() {
      this.__updateValue();
    },
    yearsCron() {
      this.__updateValue();
    },
    descriptionValue() {
      this.$emit("description-value", this.descriptionValue);
    },
    cronExpression() {
      this.$emit("input", this.cronExpression);
    }
  },
  methods: {
    __updateValue() {
      this.monthsCron = this.monthsCron.replace("@", "");
      this.daysCron.dayOfWeek = this.daysCron.dayOfWeek.replace("@", "");
      this.yearsCron = this.yearsCron.replace("@", "");
      let cronExpression =
        this.secondsCron +
        " " +
        this.minutesCron +
        " " +
        this.hoursCron +
        " " +
        this.daysCron.dayOfMonth +
        " " +
        this.monthsCron +
        " " +
        this.daysCron.dayOfWeek +
        " " +
        this.yearsCron;
      const cronResult = cron(cronExpression, presetCronValidate);
      if (cronResult.isValid()) {
        this.descriptionValue = cronstrue.toString(cronExpression, {
          locale: "es",
          dayOfWeekStartIndexZero: false
        });
      } else {
        const errorValue = cronResult.getError();
        this.descriptionValue = errorValue;
      }
      this.cronExpression = cronExpression;
    }
  },
  created() {
    const cronResult = cron(this.value, presetCronValidate);
    if (cronResult.isValid()) {
      const validValue = cronResult.getValue();
      this.secondsCron = validValue.seconds;
      this.minutesCron = validValue.minutes;
      this.hoursCron = validValue.hours;
      this.daysCron = {
        dayOfWeek: validValue.daysOfWeek,
        dayOfMonth: validValue.daysOfMonth
      };
      this.monthsCron = validValue.months;
      this.yearsCron = validValue.years;
    }
    this.__updateValue();
    this.$emit("mounted", this);
  }
};
</script>
