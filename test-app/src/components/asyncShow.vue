<template>
  <div class="show">
    <h2>{{ reslut }}</h2>
  </div>
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