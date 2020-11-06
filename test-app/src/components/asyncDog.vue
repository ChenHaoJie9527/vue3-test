<template>
  <div>
    {{ data }}
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "asyncDog",
  async setup() {
    const data = await new Promise((resolve) => {
      setTimeout(() => {
        resolve("小狗呀");
      }, 2000);
    });
    return {
        data
    }
  },
});
</script>

<style>
</style>