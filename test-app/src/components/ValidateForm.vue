<template>
  <div class="validate-from-containe">
    <slot name="default"></slot>
    <div class="submit-area" @click.prevent="onSubmit">
      <slot name="submit">
        <button type="submit" class="btn btn-primary btn-block btn-large">
          提交
        </button>
      </slot>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onUnmounted } from "vue";
import mitt from "mitt";
export const emitter = mitt();
type Func = () => boolean;
export default defineComponent({
  name: "form",
  emits: ["form-submit"],
  setup(props, context) {
    let funArr: Func[] = [];
    const onSubmit = () => {
      const reslut = funArr.map((func) => func()).every(reslut => reslut);
      context.emit("form-submit", reslut);
    };
    const cb = (func: Func | undefined) => {
      if (typeof func !== "undefined") {
        funArr.push(func);
      }
    };
    emitter.on("form-item-create", cb);
    onUnmounted(() => {
      emitter.off("form-item-create", cb);
      funArr = [];
    });
    return {
      onSubmit,
    };
  },
});
</script>

<style>
</style>