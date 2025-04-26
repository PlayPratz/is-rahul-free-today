<template>
  <v-container class="d-flex flex-column justify-content-between text-center" fluid :style="`background: ${getColour(dayType)}; height: 100%`">
    <div class="mt-2">
      <div class="bg-grey-darken-4 px-2 py-2 mx-auto">
        <h1 class="font-weight-light">
          {{ getStatus(dayType) }}
        </h1>
      </div>
      <br>
    </div>
    <DatePicker :on-update-date="onUpdateDate" />
  </v-container>
</template>

<script setup lang="ts">

  const dayType = ref(getDayType(new Date()));

  function onUpdateDate (date: Date) {
    const newDayType = getDayType(date);
    dayType.value = newDayType;
  }

  function getDayType (date: Date): number {
    const reference = new Date(2025, 2, 31);
    const millisecondgap = Math.abs(date.getTime() - reference.valueOf());
    const millisinday = 1000 * 60 * 60 * 24
    const dayGap = Math.floor(millisecondgap / millisinday) % millisinday;
    const dayType = dayGap%4;
    return dayType;
  }

  function getColour (dayType: number): string {
    switch (dayType) {
      case 0: return 'skyblue';
      case 1: return 'limegreen';
      case 2: return 'gold';
      case 3: return 'maroon';
    }
    return 'black';
  }

  function getStatus (dayType: number): string {
    switch (dayType) {
      case 0: return 'Rahul has his first day off!';
      case 1: return 'Rahul has his second day off!';
      case 2: return 'Rahul will report at 9pm.';
      case 3: return 'Rahul will work tomorrow as well :-(';
    }
    return 'error';
  }
</script>
