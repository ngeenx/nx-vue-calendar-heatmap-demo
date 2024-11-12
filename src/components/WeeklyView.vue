<template>
  <section>
    <span> Weekly </span>

    <NxCalendarHeatmap :options="options" :data="heatmapData" />
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
const heatmapData = ref<IHeatmapDay[]>([]);

const onDayClick = (day: IHeatmapDay) => {
  console.log(`Clicked on ${day.date} with value ${day.count}`);
};

const options = ref<ICalendarHeatmapOptions>({
  type: HeatMapCalendarType.WEEKLY,
  startDate: startDate.value,
  cellSize: 15,
  hideEmptyDays: false,
  colors: props.selectedColorVariant,
  onClick: onDayClick,
  tippyProps: {
    placement: 'bottom',
  },
  heatmapLegend: {
    direction: HeatmapLevelsDirection.LEFT,
  },
});

const generateHeatmapData = (startDate: DateTime) => {
  let endDate: DateTime = startDate.plus({ days: 6 });

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
    props.selectedColorVariant,
    props.selectedYear,
    props.selectedHeatmapLevelState,
    props.selectedLocale,
  ],
  () => {
    options.value = {
      ...options.value,
      colors: props.selectedColorVariant,
      startDate: startDate.value,
      locale: props.selectedLocale,
      heatmapLegend: {
        ...options.value.heatmapLegend,
        display: props.selectedHeatmapLevelState,
      },
    };

    startDate.value = DateTime.fromJSDate(
      new Date(`${props.selectedYear}-01-01`)
    ) as any;

    heatmapData.value = generateHeatmapData(startDate.value);
  }
);
onMounted(() => {
  heatmapData.value = generateHeatmapData(startDate.value);
});
</script>
