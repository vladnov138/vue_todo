<template>
  <footer class="app-footer text-center text-lg">{{ stats.active }} more to do, {{ stats.done }} done</footer>
</template>

<script lang="ts">
import { PropType, defineComponent } from "vue";

export interface Stats {
  active: number,
  done: number
}

export default defineComponent({
  name: "AppFooter",
  props: {
    stats: {
      type: Object as PropType<Stats>,
      required: true
    }
  }
});
</script>