<template>
  <div class="flex flex-col w-full h-screen">
    <div class="flex flex-row gap-5 p-3">
      <!-- Year -->
      <div class="flex flex-col w-3/12 gap-3 m-2">
        <span> Year </span>

        <select
          v-model="selectedYear"
          class="p-2 text-gray-800 border border-gray-300 rounded-md dark:text-gray-200 dark:border-gray-700 dark:bg-gray-700"
        >
          <option v-for="(year, index) in years" :key="index">
            {{ year }}
          </option>
        </select>
      </div>

      <!-- Color Variant -->
      <div class="flex flex-col w-3/12 gap-3 m-2">
        <span> Color Variant </span>

        <select
          class="p-2 text-gray-800 border border-gray-300 rounded-md dark:text-gray-200 dark:border-gray-700 dark:bg-gray-700"
          @change="onColorVariantChange($event)"
        >
          <option value="default">Default</option>
          <option
            v-for="(variant, index) in heatmapColorsVariants"
            :key="index"
            class="text-gray-800"
            :class="variant[heatmapColorsVariants.length - 1].className"
          >
            {{ variant[0].className }}
          </option>
        </select>
      </div>

      <!-- Display Heatmap Level -->
      <div class="flex flex-col w-3/12 gap-3 m-2">
        <span> Display Heatmap Level </span>

        <select
          v-model="selectedHeatmapLevelState"
          class="p-2 text-gray-800 border border-gray-300 rounded-md dark:text-gray-200 dark:border-gray-700 dark:bg-gray-700"
        >
          <option
            v-for="(option, index) in heatmapLevelDisplayOptions"
            :key="index"
            :value="Boolean(option)"
          >
            {{ option }}
          </option>
        </select>
      </div>

      <!-- Locale -->
      <div class="flex flex-col w-3/12 gap-3 m-2">
        <span>
          Locale
          <a
            href="https://moment.github.io/luxon/#/intl?id=how-locales-work"
            target="_blank"
            class="text-blue-300 underline"
            >how-locales-work?
          </a>
        </span>

        <select
          v-model="selectedLocale"
          class="p-2 text-gray-800 border border-gray-300 rounded-md dark:text-gray-200 dark:border-gray-700 dark:bg-gray-700"
        >
          <option
            v-for="(option, index) in locales"
            :key="index"
            :value="option"
          >
            {{ option }}
          </option>
        </select>
      </div>
    </div>

    <div class="flex flex-col gap-5">
      <YearlyView
        :selected-color-variant="selectedColorVariant"
        :selected-year="Number(selectedYear)"
        :selected-heatmap-level-state="selectedHeatmapLevelState"
        :selected-locale="selectedLocale"
      />

      <MonthlyView
        :selected-color-variant="selectedColorVariant"
        :selected-year="Number(selectedYear)"
        :selected-heatmap-level-state="selectedHeatmapLevelState"
        :selected-locale="selectedLocale"
      />

      <WeeklyView
        :selected-color-variant="selectedColorVariant"
        :selected-year="Number(selectedYear)"
        :selected-heatmap-level-state="selectedHeatmapLevelState"
        :selected-locale="selectedLocale"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import YearlyView from './components/YearlyView.vue';
import MonthlyView from './components/MonthlyView.vue';
import WeeklyView from './components/WeeklyView.vue';

// lib
import { IHeatmapColor } from '@ngeenx/nx-calendar-heatmap-utils';

import { DateTime } from 'luxon';
import { ref, watch } from 'vue';

const startDate = ref(DateTime.now().startOf('year'));

const selectedColorVariant = ref<IHeatmapColor[]>([]);
const heatmapColorsVariants: IHeatmapColor[][] = [
  // variant 1
  <IHeatmapColor[]>[
    {
      min: Number.NEGATIVE_INFINITY,
      max: 0,
      isDefault: true,
      className: 'custom-variant1-level-0',
    },
    { min: 1, max: 10, isDefault: false, className: 'custom-variant1-level-1' },
    {
      min: 10,
      max: 30,
      isDefault: false,
      className: 'custom-variant1-level-2',
    },
    {
      min: 30,
      max: 40,
      isDefault: false,
      className: 'custom-variant1-level-3',
    },
    {
      min: 40,
      max: 50,
      isDefault: false,
      className: 'custom-variant1-level-4',
    },
    {
      min: 50,
      max: Number.POSITIVE_INFINITY,
      isDefault: false,
      className: 'custom-variant1-level-5',
    },
  ],
  // variant 2
  <IHeatmapColor[]>[
    {
      min: Number.NEGATIVE_INFINITY,
      max: 0,
      isDefault: true,
      className: 'custom-variant2-level-0',
    },
    { min: 1, max: 10, isDefault: false, className: 'custom-variant2-level-1' },
    {
      min: 10,
      max: 30,
      isDefault: false,
      className: 'custom-variant2-level-2',
    },
    {
      min: 30,
      max: 40,
      isDefault: false,
      className: 'custom-variant2-level-3',
    },
    {
      min: 40,
      max: 50,
      isDefault: false,
      className: 'custom-variant2-level-4',
    },
    {
      min: 50,
      max: Number.POSITIVE_INFINITY,
      isDefault: false,
      className: 'custom-variant2-level-5',
    },
  ],
  // variant 3
  <IHeatmapColor[]>[
    {
      min: Number.NEGATIVE_INFINITY,
      max: 0,
      isDefault: true,
      className: 'custom-variant3-level-0',
    },
    { min: 1, max: 10, isDefault: false, className: 'custom-variant3-level-1' },
    {
      min: 10,
      max: 30,
      isDefault: false,
      className: 'custom-variant3-level-2',
    },
    {
      min: 30,
      max: 40,
      isDefault: false,
      className: 'custom-variant3-level-3',
    },
    {
      min: 40,
      max: 50,
      isDefault: false,
      className: 'custom-variant3-level-4',
    },
    {
      min: 50,
      max: Number.POSITIVE_INFINITY,
      isDefault: false,
      className: 'custom-variant3-level-5',
    },
  ],
  // variant 4
  <IHeatmapColor[]>[
    {
      min: Number.NEGATIVE_INFINITY,
      max: 0,
      isDefault: true,
      className: 'custom-variant4-level-0',
    },
    { min: 1, max: 10, isDefault: false, className: 'custom-variant4-level-1' },
    {
      min: 10,
      max: 30,
      isDefault: false,
      className: 'custom-variant4-level-2',
    },
    {
      min: 30,
      max: 40,
      isDefault: false,
      className: 'custom-variant4-level-3',
    },
    {
      min: 40,
      max: 50,
      isDefault: false,
      className: 'custom-variant4-level-4',
    },
    {
      min: 50,
      max: Number.POSITIVE_INFINITY,
      isDefault: false,
      className: 'custom-variant4-level-5',
    },
  ],
];

const selectedHeatmapLevelState = ref<boolean>(true);
const heatmapLevelDisplayOptions: boolean[] = [true, false];

/**
 * luxon source: https://moment.github.io/luxon/#/intl?id=how-locales-work
 * wikipedia: https://en.wikipedia.org/wiki/IETF_language_tag
 * stackoverflow: https://stackoverflow.com/a/38372164/6940144
 */
const selectedLocale = ref<string>('en');
const locales: string[] = ['en', 'tr', 'fr', 'de', 'ja', 'zh'];

const years = ref<number[]>(
  Array.from({ length: 30 }, (_, i) => i + 1998).reverse()
);
const selectedYear = ref(DateTime.now().year);

const onColorVariantChange = (event: any) => {
  const colorVariant = heatmapColorsVariants.find(
    (colorVariant: IHeatmapColor[]) =>
      colorVariant.find(
        (color: IHeatmapColor) => color.className === event.target.value
      )
  );

  if (colorVariant) {
    selectedColorVariant.value = colorVariant;
  } else {
    selectedColorVariant.value = [];
  }
};

watch(selectedYear, () => {
  startDate.value = DateTime.fromJSDate(
    new Date(`${selectedYear.value}-01-01`)
  ) as any;
});
</script>

<style lang="scss">
@import './style.scss';
@import '../node_modules/@ngeenx/nx-calendar-heatmap-utils/styles.css';
@import 'tippy.js/dist/tippy.css';
</style>
