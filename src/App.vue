<template>
  <div class="container">
    <h2>To-Do List</h2>
    <input class="form-control" type="text" v-model="searchText" placeholder="Search" />
    <hr />
    <TodoSimpleForm @add-todo="addTodo" />
    <div>{{ error }}</div>

    <div v-if="!filteredTodos.length">There is nothing to display</div>
    <TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
    <hr />
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item"><a class="page-link" href="#">Previous</a></li>
        <li class="page-item"><a class="page-link" href="#">1</a></li>
        <li class="page-item"><a class="page-link" href="#">2</a></li>
        <li class="page-item"><a class="page-link" href="#">3</a></li>
        <li class="page-item"><a class="page-link" href="#">Next</a></li>
      </ul>
    </nav>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';
import axios from 'axios';

export default {
  components: {
    TodoSimpleForm,
    TodoList
  },
  setup() {
    const todos = ref([]);
    const error = ref('');
    const totalPage = ref(0);
    const limit = 5;
    const page = ref(1);

    const getTodos = async () => {
      try {
        const res = await axios.get(
          `http://localhost:3000/todos?_page=${page.value}&_limit=${limit}`
        );

        const total = await axios.get('http://localhost:3000/todos');
        totalPage.value = total.data.length;

        todos.value = res.data;
      } catch (err) {
        console.log(err);
        error.value = 'Something went wrong.';
      }
    };
    getTodos();

    // async/await: 비동기 함수
    const addTodo = async (todo) => {
      error.value = '';
      try {
        const res = await axios.post('http://localhost:3000/todos', {
          subject: todo.subject,
          completed: todo.completed
        });
        todos.value.push(res.data);
      } catch (err) {
        console.log(err);
        error.value = 'Something went wrong.';
      }
    };

    // 데이터베이스 투두를 저장
    // const addTodo = (todo) => {
    //   error.value = '';
    //   axios
    //     .post('http://localhost:3000/todos', {
    //       subject: todo.subject,
    //       completed: todo.completed
    //     })
    //     .then((res) => {
    //       console.log(res);
    //       todos.value.push(res.data);
    //     })
    //     .catch((err) => {
    //       console.log(err);
    //       error.value = 'Something went wrong.';
    //     });
    // };

    const deleteTodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id;
      try {
        axios.delete('http://localhost:3000/todos/' + id);
        todos.value.splice(index, 1);
      } catch (err) {
        console.log(err);
        error.value = 'Something went wrong.';
      }
    };

    const toggleTodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id;
      try {
        axios.patch('http://localhost:3000/todos/' + id, {
          completed: !todos.value[index].completed
        });

        todos.value[index].completed = !todos.value[index].completed;
      } catch (err) {
        console.log(err);
        error.value = 'Something went wrong.';
      }
    };

    const searchText = ref('');
    const filteredTodos = computed(() => {
      if (searchText.value) {
        return todos.value.filter((todo) => {
          return todo.subject.includes(searchText.value);
        });
      }
      return todos.value;
    });

    return {
      todos,
      addTodo,
      deleteTodo,
      toggleTodo,
      searchText,
      filteredTodos,
      error,
      getTodos
    };
  }
};
</script>

<style></style>
