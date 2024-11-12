<template>
  <section>
    <span> Monthly </span>

    <div class="calendar-container">
      <NxCalendarHeatmap
        v-for="(_heatmapDataMonthly, index) in heatmapData"
        :key="index"
        :options="{
          ...options,
          startDate: months[index],
        }"
        :data="_heatmapDataMonthly"
      />
    </div>
  </section>
</template>

<script setup lang="ts">
import { NxCalendarHeatmap } from '@ngeenx/nx-vue-calendar-heatmap';
import {
  IHeatmapDay,
  HeatMapCalendarType,
  ICalendarHeatmapOptions,
  IHeatmapColor,
  HeatmapLevelsDirection,
} from '@ngeenx/nx-calendar-heatmap-utils';
import { DateTime } from 'luxon';
import { onMounted, ref, watch } from 'vue';

/**
 * Component props
 */
const props = defineProps({
  selectedColorVariant: {
    type: Array as () => IHeatmapColor[],
    default: () => [],
  },
  selectedYear: {
    type: Number,
    default: () => {
      return DateTime.now().year;
    },
  },
  selectedHeatmapLevelState: {
    type: Boolean,
    default: () => {
      return true;
    },
  },
  selectedLocale: {
    type: String,
    default: () => {
      return 'en';
    },
  },
});

const startDate = ref(DateTime.now().startOf('year'));
const heatmapData = ref<IHeatmapDay[][]>([]);

let months = Array.from(
  { length: 12 },
  (_, i): DateTime => DateTime.local().set({ month: i + 1, day: 1 })
);

const onDayClick = (day: IHeatmapDay) => {
  console.log(`Clicked on ${day.date} with value ${day.count}`);
};

const options = ref<ICalendarHeatmapOptions>({
  type: HeatMapCalendarType.MONTHLY,
  startDate: startDate.value,
  cellSize: 15,
  hideEmptyDays: false,
  colors: props.selectedColorVariant,
  onClick: onDayClick,
  heatmapLegend: {
    display: props.selectedHeatmapLevelState,
    direction: HeatmapLevelsDirection.RIGHT,
  },
  overWritedDayStyle: {
    borderRadius: '50%',
  },
  locale: 'en',
});

const generateHeatmapData = (startDate: DateTime) => {
  let endDate: DateTime = startDate.endOf('month');

  const daysBetween = Math.floor(endDate.diff(startDate, 'days').days);
  const heatmap = [];

  let currentDate = startDate;

  for (let i = 0; i <= daysBetween; i++) {
    const day: IHeatmapDay = {
      date: currentDate,
      count: Math.floor(Math.random() * 101),
    };

    heatmap.push(day);

    currentDate = currentDate.plus({ days: 1 });
  }

  return heatmap;
};

watch(
  () => [
    props.selectedYear,
    props.selectedColorVariant,
    props.selectedHeatmapLevelState,
    props.selectedLocale,
  ],
  () => {
    startDate.value = DateTime.fromJSDate(
      new Date(`${props.selectedYear}-01-01`)
    ) as any;

    options.value = {
      ...options.value,
      startDate: startDate.value,
      colors: props.selectedColorVariant,
      locale: props.selectedLocale,
      heatmapLegend: {
        ...options.value.heatmapLegend,
        display: props.selectedHeatmapLevelState,
      },
    };

    months = Array.from(
      { length: 12 },
      (_, i): DateTime =>
        DateTime.local().set({
          year: Number(props.selectedYear),
          month: i + 1,
          day: 1,
        })
    );

    heatmapData.value = months.map((firstDayOfMonth: DateTime) => {
      return generateHeatmapData(firstDayOfMonth);
    });
  }
);

onMounted(() => {
  heatmapData.value = months.map((firstDayOfMonth: DateTime) => {
    return generateHeatmapData(firstDayOfMonth);
  });
});
</script>

<style>
.calendar-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  margin-top: 20px;
}
</style>
