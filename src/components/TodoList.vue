<template>
  <div v-for="(todo, index) in todos" :key="todo.id" class="card mt-2">
    <div class="card-body p-2 d-flex align-items-center">
      <div class="form-check flex-grow-1">
        <!-- props -->
        <input
          class="form-check-input"
          type="checkbox"
          :value="todo.completed"
          @change="toggleTodo(index)"
        />

        <!-- style 바인딩 -->
        <!-- <label class="form-check-label" :style="todo.completed ? todoStyle : {}">{{
            todo.subject
          }}</label> -->

        <!-- class 바인딩 -->
        <label class="form-check-label" :class="{ todo: todo.completed }">{{ todo.subject }}</label>
      </div>
      <div>
        <button class="btn btn-danger btn-sm" @click="deleteTodo(index)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // props: 부모에서 자식
  props: {
    todos: {
      type: Array,
      required: true
    }
  },
  emits: ['toggle-todo', 'delete-todo'],
  setup(props, { emit }) {
    // emit: 자식에서 부모
    const toggleTodo = (index) => {
      emit('toggle-todo', index);
    };

    const deleteTodo = (index) => {
      emit('delete-todo', index);
    };

    return {
      toggleTodo,
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
