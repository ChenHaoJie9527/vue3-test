<template>
  <div class="show">{{ reslut }}</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "asyncShow",
  setup() {
    return new Promise((reselve) => {
      setTimeout(() => {
        reselve({
          reslut: 40,
        });
      }, 3000);
    });
  },
});
</script>

<style>
</style>