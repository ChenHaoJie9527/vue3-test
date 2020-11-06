<template>
  <teleport to="#model">
    <div class="dialog" v-if="isOpen">
      <p>
        <slot>this is a model</slot>
      </p>
      <button @click.prevent="onCloseModal">close</button>
    </div>
  </teleport>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "dialog",
  props: {
    isOpen: Boolean,
  },
  emits: ["close-model"],
  setup(props, context) {
    const onCloseModal = () => {
      context.emit("close-model");
    };
    return {
      onCloseModal,
    };
  },
});
</script>

<style scoped>
.dialog {
  width: 200px;
  height: 200px;
  border: 2px #ccc solid;
  background: white;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -100px;
  margin-top: -100px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}
</style>