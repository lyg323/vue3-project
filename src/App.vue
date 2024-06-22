<template>
  <div class="container">
    <h2>To-Do List</h2>

    <!-- @submit.prevent : 리로드 방지 -->
    <form @submit.prevent="onSubmit">
      <div class="d-flex">
        <div class="flex-grow-1 mr-2">
          <!-- v-model : 양방향 바인딩 -->
          <input class="form-control" type="text" v-model="todo" placeholder="Type new to-do" />
        </div>
        <div>
          <button class="btn btn-primary" type="submit">Add</button>
        </div>
      </div>

      <!-- 
        v-show vs v-if
        v-show : 자주 Toggle
        v-if : 자주 Toggle X (렌더링 이슈)
       -->
      <div v-show="hasError" style="color: red">This field cannot be empty</div>
    </form>

    <div v-if="!todos.length">추가된 Todo가 없습니다</div>

    <!-- v-for : 반복문 -->
    <div v-for="(todo, index) in todos" :key="todo.id" class="card mt-2">
      <div class="card-body p-2 d-flex align-items-center">
        <div class="form-check flex-grow-1">
          <!-- 체크박스 바인딩 -->
          <input class="form-check-input" type="checkbox" v-model="todo.completed" />

          <!-- class & style 바인딩 -->
          <!-- <label class="form-check-label" :style="todo.completed ? todoStyle : {}">{{
            todo.subject
          }}</label> -->
          <label class="form-check-label" :class="{ todo: todo.completed }">{{
            todo.subject
          }}</label>
        </div>
        <div>
          <!-- v-on : 이벤트 -->
          <button class="btn btn-danger btn-sm" @click="deleteTodo(index)">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    // ref
    const todo = ref('');
    const todos = ref([]);
    const hasError = ref(false);
    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray'
    };

    const onSubmit = () => {
      // 빈값 체크 후 에러 텍스트 노출
      if (todo.value === '') {
        hasError.value = true;
      } else {
        // 리스트에 새로운 todo 객체 추가
        todos.value.push({
          id: Date.now(),
          subject: todo.value,
          completed: false
        });
        hasError.value = false;
        todo.value = '';
      }
    };

    // 배열에서 아이템 삭제
    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    };

    return {
      todo,
      todos,
      hasError,
      todoStyle,
      onSubmit,
      deleteTodo
    };
  }
};
</script>

<style>
.todo {
  color: gray;
  text-decoration: line-through;
}
</style>
