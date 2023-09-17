<template>
  <div class="p-8 flex flex-col gap-8 items-start">
    <form class="flex flex-row gap-4 items-end" @submit.prevent="generate">
      <div>
        <label
          for="width"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Width</label
        >
        <div class="mt-2">
          <input
            type="number"
            name="width"
            id="width"
            autocomplete="off"
            v-model="width"
            class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          />
        </div>
      </div>

      <div>
        <label
          for="height"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Height</label
        >
        <div class="mt-2">
          <input
            type="number"
            name="height"
            id="height"
            autocomplete="off"
            v-model="height"
            class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          />
        </div>
      </div>

      <div>
        <label
          for="squares"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Squares</label
        >
        <div class="mt-2">
          <input
            type="number"
            name="squares"
            id="squares"
            autocomplete="off"
            v-model="squares"
            class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          />
        </div>
      </div>

      <button
        type="submit"
        class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
      >
        Generate
      </button>
    </form>

    <div
      class="grid grid-cols-[--width] gap-2"
      :style="{ '--width': `repeat(${width}, 1fr)` }"
    >
      <div
        v-for="(number, index) in gridNumbers"
        :key="'' + number + index"
        class="w-8 h-8 rounded flex items-center justify-center bg-gray-100 text-gray-800"
      >
        <span class="leading-3 font-semibold">{{ number + 1 }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const width = ref(15);
const height = ref(10);
const squares = ref(9);

function randomSquare() {
  return Math.floor(Math.random() * squares.value);
}

const gridNumbers = ref<number[]>([]);

watch([width, height, squares], () => generate());

function generate() {
  const grid = Array.from({ length: width.value * height.value }, () =>
    randomSquare()
  );

  console.log(grid);

  for (let i = 0; i < grid.length; i++) {
    const gridX = i % width.value;
    const gridY = (i - gridX) / width.value;

    const top = gridY < height.value - 1 ? grid[i + width.value] : null;
    const bottom = gridY > 0 ? grid[i - width.value] : null;
    const left = gridX > 0 ? grid[i - 1] : null;
    const right = gridX < width.value ? grid[i + 1] : null;

    const filled: number[] = [top, bottom, left, right].filter(
      (item): item is number => item !== null
    );

    while (filled.includes(grid[i])) {
      grid[i] = randomSquare();
    }
  }

  gridNumbers.value = grid;
}

onMounted(() => {
  generate();
});
</script>
