<template>
  <div class="container">
    <h2>To-Do List</h2>
    <form class="d-flex" @submit.prevent="onSubmit">
      <div class="flex-grow-1 mr-2">
        <input class="form-control" type="text" v-model="todo" placeholder="Type new to-do" />
      </div>
      <div>
        <button class="btn btn-primary" type="submit">Add</button>
      </div>
    </form>
    {{ todos }}
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const todo = ref('');
    const todos = ref([]);

    const onSubmit = (e) => {
      // e.preventDefault();
      // @submit.prevent와 같음

      // 객체 추가
      todos.value.push({
        id: Date.now(),
        subject: todo.value
      });
    };

    return {
      todo,
      todos,
      onSubmit
    };
  }
};
</script>

<style></style>
