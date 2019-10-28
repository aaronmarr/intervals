<template>
  <div class="max-w-full h-full">
    <div class="sticky z-50 top-0 px-4 md:px-6 py-4 flex flex-row text-white bg-gray-900 shadow-md">
      <div class="mb-1 pr-4">
        <div>
          <div class="inline-block relative">
            <label class="block text-gray-500 text-sm font-normal mb-1" for="scale">
              Interval
            </label>
            <div class="relative">
              <select 
                id="scale" 
                v-model="selectedScale" 
                class="h-10 block appearance-none w-full bg-gray-900 border border-gray-600 hover:border-gray-300 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline text-base text-gray-400"
              >
                <option 
                  v-for="interval in scale" 
                  v-bind:value="interval.value" 
                  v-bind:key="interval.label"
                >
                  {{ interval.label }}
                </option>
              </select>
              <div 
                class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-400"
              >
                <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="mb-1">
        <div>
          <label class="block text-gray-500 text-sm font-normal mb-1" for="base">
            Base font size
          </label>
          <input 
            class="h-10 shadow appearance-none bg-gray-900 border border-gray-600 hover:border-gray-300 rounded w-full py-2 px-3 text-base text-gray-400 leading-tight focus:outline-none focus:shadow-outline" 
            id="base" 
            type="text" 
            placeholder="1rem"
            v-model="base"
          >
        </div>
      </div>
    </div>
    <div class="overflow-x-hidden">
      <div 
        v-bind:key="item.fontSize" 
        v-for="item in items" 
        class="relative items-center flex whitespace-no-wrap px-4 md:px-6 py-8 border-b bg-gray-200 border-gray-300" 
      >
        <span class="font-mono text-sm absolute mt-2 top-0 leading-snug bg-gray-200 text-gray-600 px-0 py-0">
          {{ item.fontSize }}{{ unit }}
        </span>
        <span 
          class="leading-snug text-gray-800 transition-all" 
          v-bind:style="{ 'font-size': item.fontSize + unit }"
        >
          The quick brown fox jumps over the lazy dog
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  props: {
    msg: String
  },
  computed: {
    unit() {
      return this.base.replace(/[^A-Za-z]+/g, '');
    },
    items() {
      const baseSize = parseFloat(this.base);
      
      const items = Array(8).fill(0).reduce(function(accumulator, currentValue, i) {
        const fontSize = parseFloat(
          Number(
            (
              (accumulator[i - 1] && accumulator[i - 1].fontSize || baseSize) * this.selectedScale
            ).toFixed(4)
          )
        );
        
        accumulator.push({
          fontSize,
        });

        return accumulator;
      }.bind(this), [])
      
      items.unshift({ fontSize: baseSize });
      items.unshift({ fontSize: baseSize - Math.abs(baseSize - this.selectedScale )});
      items.reverse();
      
      return items;
    },
  },
  data: () => ({
    base: '1rem',
    selectedScale: 1.067,
    scale: {
      minorSecond: {
        label: 'Minor Second',
        value: 1.067,
      },
      majorSecond: {
        label: 'Major Second',
        value: 1.125,
      },
      minorThird: {
        label: 'Minor Third',
        value: 1.2,
      },
      majorThird: {
        label: 'Major Third',
        value: 1.25,
      },
      perfectFourth: {
        label: 'Perfect Fourth',
        value: 1.333,
      },
      augmentedFourth: {
        label: 'Augmented Fourth',
        value: 1.414,
      },
      perfectFifth: {
        label: 'Perfect Fifth',
        value: 1.5,
      },
      minorSixth: {
        label: 'Minor Sixth',
        value: 1.6,
      },
      goldenRatio: {
        label: 'Golden Ratio',
        value: 1.618,
      },
      majorSixth: {
        label: 'Major Sixth',
        value: 1.667,
      },
      minorSeventh: {
        label: 'Minor Seventh',
        value: 1.778,
      },
      majorSeventh: {
        label: 'Major Seventh',
        value: 1.875,
      },
      octave: {
        label: 'Octave',
        value: 2,
      },
    },
  })
}
</script>
