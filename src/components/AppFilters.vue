<template>
    <aside class="app-filters">
        <section class="flex justify-center items-center space-x-2">
          <button 
            class="button" 
            v-for="filter in filters" 
            :key="filter"
            :class="{'button--primary text-3xl font-bold underline': activeFilter === filter}"
            @click="setFilter(filter)">{{ filter }}</button>
        </section>
      </aside>
</template>

<script lang="ts">
import { Filter } from "@/types/Filter";
import { PropType, defineComponent } from "vue";

interface State {
  filters: Filter[]
}

export default defineComponent({
  name: "AppFilters",
  props: {
    activeFilter: {
      type: String as PropType<Filter>,
      required: true,
    }
  },
  data(): State {
    return {
      filters: ['All', "Active", "Done"]
    }
  },
  methods: {
    setFilter(filter: Filter) {
      this.$emit('setFilter', filter)
    }
  },
  emits: {
    setFilter: (filter: Filter) => filter
  }
});
</script>