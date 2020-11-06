<template>
  <div id="app">
    <p>{{ val }}</p>
    <p>{{ double }}</p>
    <ul>
      <li v-for="item in arrs" :key="item">
        <h2>{{ item }}</h2>
      </li>
    </ul>
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
    <Dialog :isOpen="modelisOpen" @close-model="closeModel">
      <h2>this is model!</h2>
    </Dialog>
    <button @click.prevent="openModel">open</button>
    <h2>{{ persons.name }}</h2>
    <button @click.prevent="oncreet">点击+1</button>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, reactive, ref, toRefs } from "vue";
interface DataProps {
  val: number;
  double: number;
  oncreet: () => void;
  arrs: Array<number>;
  persons: { name?: string };
}
import Dialog from "./components/Dialog.vue";
import asyncShow from "./components/asyncShow.vue";
import defaulShow from "./components/defaulShow.vue";
import asyncDog from "./components/asyncDog.vue";
export default defineComponent({
  name: "App",
  components: {
    Dialog,
    asyncShow,
    defaulShow,
    asyncDog,
  },
  setup() {
    // const val = ref(0);
    // const double = computed(()=>{
    //   return val.value * 2;
    // })
    // const oncreet = () => {
    //   val.value++;
    // };
    const modelisOpen = ref(false);
    const data: DataProps = reactive({
      val: 0,
      double: computed(() => {
        return data.val * 2;
      }),
      oncreet: () => {
        data.val++;
      },
      arrs: [1, 2, 3, 4, 5],
      persons: {},
    });

    data.arrs[0] = 10;
    data.persons.name = "vikeet";
    const rawdata = toRefs(data);
    const openModel = () => {
      modelisOpen.value = true;
    };
    const closeModel = () => {
      modelisOpen.value = false;
    };
    return {
      ...rawdata,
      modelisOpen,
      openModel,
      closeModel,
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
