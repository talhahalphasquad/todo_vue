<template id="app">
  <div
    class="w-full bg-cover bg-no-repeat bg-white/30 backdrop-grayscale-200 h-[400px]"
    style="background-image: url(./src/img/dbg.jpeg); height: 400px"
  >
    <h2
      class="relative z-20 font-black text-center 2xl:text-7xl xl:text-6xl lg:text-5xl md:text-4xl sm:text-3xl text-3xl pt-15 xl:pt-32 text-black dark:text-white font-sans tracking-tight"
    >
      Todo App in
      <div
        class="relative mx-auto inline-block w-max [filter:drop-shadow(0px_1px_3px_rgba(27,_37,_80,_0.14))]"
      >
        <div
          class="absolute left-0 top-[1px] bg-clip-text bg-no-repeat text-transparent bg-gradient-to-r py-4 from-emerald-700 via-emerald-300 to-emerald-600 [text-shadow:0_0_rgba(0,0,0,0.1)]"
        >
          <span className="">Vue 3</span>
        </div>
        <div
          class="relative bg-clip-text text-transparent bg-no-repeat bg-gradient-to-r from-emerald-700 via-emerald-300 to-emerald-600 py-4"
        >
          <span className="">Vue 3</span>
        </div>
      </div>
    </h2>
    <div
      className="flex overflow-hidden justify-center p-6 text-sm md:text-md 2xl:text-lg "
    >
      <input
        className="p-2 m-2 lg:p-4 lg:m-4 rounded-l-xl border-2 border-emerald-500 outline-emerald-500 bg-zinc-800/10 backdrop-blur-xl border-opacity-20 text-stone-200 placeholder-opacity-20 xl:w-2xl sm:w-2xs w-full"
        v-model="newTodo.text"
        @keyup.enter="addTodo"
        type="text"
        placeholder="Add a new task"
        maxlength="50"
      />
      <input
        type="time"
        v-model="newTodo.time"
        @keyup.enter="addTodo"
        className="p-2 m-2 lg:p-4 lg:m-4 accent-emerald-500 border-2 border-emerald-500 outline-emerald-500 bg-emerald-300 text-stone-800 md:text-md 2xl:text-lg backdrop-blur-xl border-opacity-20 placeholder-opacity-20 w-[125px] xl:w-[150px]"
      />
      <button
        @click="addTodo"
        className=" bg-emerald-300 text-stone-800 hover:bg-emerald-500 hover:text-black p-4 rounded-r-3xl border-2 text-xs border-emerald-500 drop-shadow-lg border-opacity-20"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="icon icon-tabler icons-tabler-outline icon-tabler-plus"
        >
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <path d="M12 5l0 14" />
          <path d="M5 12l14 0" />
        </svg>
      </button>
    </div>
  </div>
  <div class="w-full flex justify-center relative -top-10">
    <div
      className="text-center w-full bg-stone-900/5 backdrop-blur-sm xl:w-3xl text-sm md:w-md sm:w-sm text-white ring-1 ring-emerald-600 rounded-2xl p-4 mt-[50px]"
    >
      <p v-if="todos.length === 0">No Tasks !</p>

      <ul
        className="grid grid-cols gap-4 list-none w-full rounded-xl 2xl:text-lg xl:text-lg lg:text-md md:text-sm sm-text-sm xs-text-sm"
      >
        <div class="flex justify-center space-x-4 mt-4">
          <button
            @click="filterTodos('all')"
            class="p-2 rounded-xl bolder hover:text-zinc-400 text-white"
          >
            All
          </button>
          <button
            @click="filterTodos('active')"
            class="p-2 rounded-xl bolder hover:text-zinc-400 text-white"
          >
            Active
          </button>
          <button
            @click="filterTodos('completed')"
            class="p-2 rounded-xl bolder hover:text-zinc-400 text-white"
          >
            Completed
          </button>
          <button
            @click="clearCompleted"
            class="p-2 rounded-xl bolder hover:text-zinc-400 text-white"
          >
            Clear Completed
          </button>
        </div>
        <li
          v-for="(todo, index) in filteredTodos"
          :key="index"
          className="flex overflow-hidden text-wrap justify-between bg-zinc-850 hover:bg-zinc-950 border-emerald-600 hover:border-emerald-800 p-4 border-1 rounded-xl"
        >
          <input
            type="checkbox"
            v-model="todo.completed"
            className="accent-emerald-500 rounded-xl size-4 mr-2 self-center hover:accent-emerald-600"
          />
          <div
            :class="{ completed: todo.completed }"
            className="mx-8 self-center"
          >
            {{ todo.text }}
            <span v-if="todo.time" class="text-sm text-gray-400 ml-2"
              >({{ todo.time }})</span
            >
          </div>
          <button @click="removeTodo(index)" className="hover:text-red-400">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="icon icon-tabler icons-tabler-outline icon-tabler-trash"
            >
              <path stroke="none" d="M0 0h24v24H0z" fill="none" />
              <path d="M4 7l16 0" />
              <path d="M10 11l0 6" />
              <path d="M14 11l0 6" />
              <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
              <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
            </svg>
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
  name: "App",

  setup() {
    const newTodo = ref({
      text: "",
      time: "",
    });
    const todos = ref([]);
    const filter = ref("all");

    const addTodo = () => {
      if (newTodo.value.text.trim()) {
        todos.value.push({
          text: newTodo.value.text,
          time: newTodo.value.time,
          completed: false,
          return: console.log(newTodo),
        });
        localStorage.setItem("items", JSON.stringify(todos.value));
        newTodo.value.text = "";
        newTodo.value.time = "";
      }
    };

    const removeTodo = (index) => {
      todos.value.splice(index, 1);
      localStorage.setItem("items", JSON.stringify(todos.value));
    };

    const activeTodos = computed(() => {
      return todos.value.filter((todo) => !todo.completed).length;
    });

    const completedTodos = computed(() => {
      return todos.value.filter((todo) => todo.completed).length;
    });

    const filteredTodos = computed(() => {
      if (filter.value === "active") {
        return todos.value.filter((todo) => !todo.completed);
      } else if (filter.value === "completed") {
        return todos.value.filter((todo) => todo.completed);
      }
      return todos.value;
    });

    const filterTodos = (status) => {
      filter.value = status;
    };

    const clearCompleted = () => {
      todos.value = todos.value.filter((todo) => !todo.completed);
      localStorage.setItem("items", JSON.stringify(todos.value));
    };

    return {
      newTodo,
      todos,
      addTodo,
      removeTodo,
      activeTodos,
      completedTodos,
      filteredTodos,
      filterTodos,
      clearCompleted,
    };
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  color: rgb(124, 124, 124);
}
</style>
