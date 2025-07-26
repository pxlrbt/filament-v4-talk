<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps({
  page: {
    default: '',
  },
  contained: {
    type: Boolean,
    default: true,
  },
  width: {
    type: String,
    default: '100%',
  },
  height: {
    type: String,
    default: '20rem',
  },

})

</script>

<template>
<div
   class="overflow-hidden"
   :class="{'contained': props.contained}"
>
  <div :style="{'width': props.width, 'height': props.height}">
    <iframe
      :src="'http://filament-talk-demo.test/demo/'+props.page"
      class="max-w-full overflow-hidden"

      :style="{'width': props.width, 'height': props.height}"
    />
  </div>
</div>

</template>

<style scoped>
.contained {
  border: 1px solid rgba(238, 238, 238, 0.2);
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(255, 255, 255, 0.1);
}
</style>
