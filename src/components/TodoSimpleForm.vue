<template>
  <!-- @submit.prevent : 리로드 방지 -->
  <form @submit.prevent="onSubmit">
    <div class="d-flex">
      <div class="flex-grow-1 me-2">
        <input class="form-control" type="text" v-model="todo" placeholder="Type new to-do" />
      </div>
      <div>
        <button class="btn btn-primary" type="submit">Add</button>
      </div>
    </div>

    <div v-show="hasError" style="color: red">This field cannot be empty</div>
  </form>
</template>

<script>
import { ref } from 'vue';

export default {
  emits: ['add-todo'],
  setup(props, { emit }) {
    const todo = ref('');
    const hasError = ref(false);

    const onSubmit = () => {
      // 빈값 체크
      if (todo.value === '') {
        hasError.value = true;
      }
      // 리스트에 새로운 todo 객체 추가
      else {
        // todos.value.push({
        //   id: Date.now(),
        //   subject: todo.value,
        //   completed: false
        // });

        // emit: 자식에서 부모
        emit('add-todo', {
          id: Date.now(),
          subject: todo.value,
          completed: false
        });

        hasError.value = false;
        todo.value = '';
      }
    };

    return {
      todo,
      hasError,
      onSubmit
    };
  }
};
</script>

<style></style>
