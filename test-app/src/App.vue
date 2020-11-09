<template>
  <div class="container">
    <Suspense>
      <!--default默认渲染异步组件的内容-->
      <template #default>
        <div>
          <asyncShow></asyncShow>
          <asyncDog></asyncDog>
        </div>
      </template>
      <!--fallback渲染异步组件尚未加载完成之前的内容-->
      <template #fallback>
        <defaulShow></defaulShow>
      </template>
    </Suspense>
    <ValidateForm @form-submit="onSubmit">
      <div class="mb-3">
        <label class="form-label">邮箱地址</label>
        <ValidateInput
          :rules="emailRules"
          v-model="emailVal"
          placeholder="请输入电子邮箱地址"
          type="text"
        ></ValidateInput>
      </div>
      <div class="mb-3">
        <label class="form-label">密码</label>
        <ValidateInput
          :rules="passwordRules"
          v-model="passwordVal"
          placeholder="请输入密码"
          type="password"
        ></ValidateInput>
      </div>
      <div class="bm-3">
        <label class="form-label">重复密码</label>
        <ValidateInput
          placeholder="请再输入一次密码"
          v-model="repeatPsswordValue"
          :rules="repeatPsswordRules"
          type="password"
        ></ValidateInput>
      </div>
      <template #submit>
        <span class="btn btn-danger">Submit</span>
      </template>
    </ValidateForm>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import ValidateForm from "./components/ValidateForm.vue";
interface DataProps {
  val: number;
  double: number;
  oncreet: () => void;
  arrs: Array<number>;
  persons: { name?: string };
}
import asyncShow from "./components/asyncShow.vue";
import defaulShow from "./components/defaulShow.vue";
import asyncDog from "./components/asyncDog.vue";
import "bootstrap/dist/css/bootstrap.min.css";
import ValidateInput, { RulesProps } from "./components/ValidateInpute.vue";
export default defineComponent({
  name: "App",
  components: {
    asyncShow,
    defaulShow,
    asyncDog,
    ValidateInput,
    ValidateForm,
  },
  setup() {
    const emailVal = ref("");
    const passwordVal = ref("");
    const repeatPsswordValue = ref("");
    const emailRules: RulesProps = [
      {
        type: "required",
        message: "电子邮箱不能为空",
      },
      {
        type: "email",
        message: "请输入正确的电子邮箱格式",
      },
    ];
    const passwordRules: RulesProps = [
      {
        type: "required",
        message: "密码不能为空",
      },
      {
        type: "password",
        message: "密码格式不能包含特殊字符",
      },
    ];
    const repeatPsswordRules: RulesProps = [
      {
        type: "comstron",
        message: "密码不一致",
        invalidate: () => {
          return passwordVal.value == repeatPsswordValue.value;
        },
      },
      {
        type:"required",
        message: "密码不能为空"
      }
    ];
    const onSubmit = (val: boolean)=> {
      console.log(val);
    }
    return {
      emailRules,
      passwordRules,
      emailVal,
      passwordVal,
      repeatPsswordValue,
      repeatPsswordRules,
      onSubmit,
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
