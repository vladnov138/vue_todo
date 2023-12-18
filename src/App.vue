<template>
  <AppHeaderVue />
  <AppFiltersVue :active-filter="activeFilter" @set-filter="setFilter"/>

  <main class="app-main">
    <AppTodoListVue :todos="filteredTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo"/>
    <AppAddTodoVue @add-todo="addTodo"/>
    <AppResetStorage @reset-storage="resetData"/>
  </main>

  <AppFooterVue :stats="stats"/>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import AppAddTodoVue from "./components/AppAddTodo.vue";
import AppFiltersVue from "./components/AppFilters.vue";
import AppFooterVue, { Stats } from "./components/AppFooter.vue";
import AppHeaderVue from "./components/AppHeader.vue";
import AppTodoListVue from "./components/AppTodoList.vue";
import AppResetStorage from './components/AppResetStorage.vue';
import { Todo } from "./types/Todo";
import { Filter } from "./types/Filter";

interface State {
  todos: Todo[],
  activeFilter: Filter,
}

export default defineComponent({
  components: { AppHeaderVue, AppFiltersVue, AppTodoListVue, AppAddTodoVue, AppFooterVue, AppResetStorage },
  data(): State {
    return {
      todos: [
        { id: 0, text: 'Learn the basics of Vue', completed: true },
        { id: 1, text: 'Learn the basics of Typescript', completed: false },
        { id: 2, text: 'Learn the basics of tailwindcss', completed: false },
      ],
      activeFilter: 'All'
    }
  },
  mounted() {
    const storedData = sessionStorage.getItem('todos');
    if (storedData) {
      this.todos = JSON.parse(storedData);
    }
  },
  computed: {
    filteredTodos(): Todo[] {
      switch (this.activeFilter) {
        case 'Active':
          return this.activeTodos;
        case 'Done':
          return this.doneTodos;
        case 'All':
        default:
          return this.todos;
      }
    },
    stats(): Stats {
      return {
        active: this.activeTodos.length,
        done: this.doneTodos.length,
      }
    },
    activeTodos(): Todo[] {
      return this.todos.filter(todo => !todo.completed)
    },
    doneTodos(): Todo[] {
      return this.todos.filter(todo => todo.completed);
    }
  },
  methods: {
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id)
      if (targetTodo) {
        targetTodo.completed = !targetTodo.completed
      }
      sessionStorage.setItem('todos', JSON.stringify(this.todos));
    },
    resetData() {
      sessionStorage.removeItem('todos');
      this.todos = [];
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo: Todo) => todo.id !== id)
      sessionStorage.setItem('todos', JSON.stringify(this.todos));
    },
    addTodo(todo: Todo) {
      this.todos.push(todo);
      sessionStorage.setItem('todos', JSON.stringify(this.todos));
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter;
    }
  }
});
</script>


