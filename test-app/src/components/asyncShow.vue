<template>
  <div class="show">
    <h2>{{ data.reslut }}</h2>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "asyncShow",
  async setup() {
    const data = await new Promise((reselve) => {
      setTimeout(() => {
        reselve({
          reslut: 40,
        });
      }, 1000);
    });
    return {
      data
    }
  },
});
</script>

<style>
</style>