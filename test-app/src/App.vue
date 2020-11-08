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
    <form action="">
      <div class="mb-3">
        <label class="form-label">邮箱地址</label>
        <ValidateInput :rules="emailRules"></ValidateInput>
      </div>
      
    </form>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
} from "vue";
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
import 'bootstrap/dist/css/bootstrap.min.css';
import ValidateInput, {RulesProps} from "./components/ValidateInpute.vue";
export default defineComponent({
  name: "App",
  components: {
    asyncShow,
    defaulShow,
    asyncDog,
    ValidateInput
  },
  setup() {
    const emailRules: RulesProps = [
      {
        type: "required",
        message: "电子邮箱不能为空"
      },
      {
        type:"email",
        message: "请输入正确的电子邮箱格式"
      }
    ];

    return {
      emailRules
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
