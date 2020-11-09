<template>
  <div class="validate-input-container pb-3">
    <input
      class="form-control"
      :class="{ 'is-invalid': inputRef.error }"
      :value="inputRef.val"
      @input="updateValue"
      @blur="validateInput"
      v-bind="$attrs"
    />
    <span v-if="inputRef.error" class="invalid-feedback">
      {{ inputRef.message }}
    </span>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, PropType, reactive } from "vue";
import { emitter } from "./ValidateForm.vue";
interface RuleProp {
  type: "required" | "email" | "password" | "comstron";
  message: string;
  invalidate?: () => boolean;
}
export type RulesProps = RuleProp[];
export default defineComponent({
  name: "input-item",
  props: {
    rules: {
      type: Array as PropType<RulesProps>,
    },
    modelValue: String,
  },
  inheritAttrs: false,
  setup(props, context) {
    const inputRef = reactive({
      val: (props.modelValue as string) || "",
      error: false,
      message: "",
    });
    const emailReg = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
    const passwordReg = /^[A-Za-z0-9]+$/;
    const updateValue = (e: KeyboardEvent) => {
      const targetValue = (e.target as HTMLInputElement).value;
      inputRef.val = targetValue;
      context.emit("update:modelValue", targetValue);
    };
    const validateInput = () => {
      if (props.rules) {
        const allPassed = props.rules.every((rule) => {
          let passed = true;
          inputRef.message = rule.message;
          switch (rule.type) {
            case "required":
              passed = inputRef.val.trim() !== "";
              break;
            case "email":
              passed = emailReg.test(inputRef.val);
              break;
            case "password":
              passed = passwordReg.test(inputRef.val);
              break;
            case "comstron":
              passed = rule.invalidate ? rule.invalidate() : true;
              break;
            default:
              break;
          }
          return passed;
        });
        inputRef.error = !allPassed;
        return allPassed;
      }
      return true;
    };
    onMounted(()=>{
      emitter.emit("form-item-create", validateInput);
    })
    return {
      inputRef,
      validateInput,
      updateValue,
    };
  },
});
</script>

<style>
</style>