<template>
  <p>{{ name1 }}</p>
  <p>{{ name2 }}</p>
  <p>{{ name3 }}</p>

  <button class="btn btn-primary" v-on:click="updateName">Click</button>
</template>

<script>
import { ref, reactive } from 'vue';

export default {
  setup() {
    // 함수
    // const name = 'test';
    // const greeting = (name) => {
    //   return 'Hello, ' + name
    // }
    // const greet = greeting(name)

    // return {
    //   greet
    // }

    // ref, reactive
    const name1 = ref('test');
    const name2 = ref({
      id: 1
    });
    const name3 = reactive({
      id: 1
    });

    const updateName = () => {
      // ref : number, string
      name1.value = 'test test';

      // ref : object, array
      name2.value.id = '2';

      // reactive : object, array
      name3.id = '2';
    };

    return {
      name1,
      name2,
      name3,
      updateName
    };
  }
};
</script>

<style></style>
