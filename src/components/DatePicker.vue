<template>

  <v-date-picker
    class="mx-auto"
    :min="`${REFERENCE_DATE.getFullYear()}-${REFERENCE_DATE.getMonth()+1}-${REFERENCE_DATE.getDate()}`"
    :model-value="selectedDate"
    @update:model-value="updateDate"
  />

  <div class="my-2 mx-10 px-2 d-flex" style="justify-content: space-between;">
    <v-btn :disabled="isLowerLimit(selectedDate)" icon="mdi-arrow-left" @click="previousDate" />
    <v-btn icon="mdi-arrow-right" @click="nextDate" />
  </div>
</template>

<script setup lang="ts">

  import { REFERENCE_DATE } from '../stores/app.ts';

  const props = defineProps<{ onUpdateDate: (date: Date) => void }>();

  const selectedDate = ref(new Date());

  function previousDate () {
    if(isLowerLimit(selectedDate.value)) return;

    const date = new Date(selectedDate.value.getFullYear(),
                          selectedDate.value.getMonth(),
                          selectedDate.value.getDate() - 1);
    updateDate(date);
  }

  function nextDate () {
    const date = new Date(selectedDate.value.getFullYear(),
                          selectedDate.value.getMonth(),
                          selectedDate.value.getDate() + 1);
    updateDate(date);
  }

  function updateDate (date: object) {
    if(date instanceof Date) {
      selectedDate.value = date;
      props.onUpdateDate(date);
    }
  }

  function isLowerLimit (date: Date): boolean {
    return date.getFullYear() === REFERENCE_DATE.getFullYear()
      && date.getMonth() === REFERENCE_DATE.getMonth()
      && date.getDate() === REFERENCE_DATE.getDate();
  }

  addEventListener('keydown', _ => {
    if(_.key === 'ArrowLeft') {
      previousDate()
    } else if (_.key === 'ArrowRight') {
      nextDate()
    }
  } );
</script>
