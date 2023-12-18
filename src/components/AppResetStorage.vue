<template>
  <section class="add-todo grid">
    <button class="items-center add-todo__show-form-button" @click="resetStorage">
      Reset storage
    </button>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "AppResetStorage",
  methods: {
    resetStorage() {
      this.$emit('resetStorage');
    }
  },
  emits: {
    resetStorage: () => true
  }
});
</script>