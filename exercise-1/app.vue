<template>
  <div>
    <img src="/todo.jpg" alt="todo photo by Ionela Mat" />
    <p>
      Photo by
      <a
        href="https://unsplash.com/@ionelaaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText"
        >Ionela Mat</a
      >
      on
      <a
        href="https://unsplash.com/photos/DQzGtBbtMa8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText"
        >Unsplash</a
      >
    </p>
    <h1>HELLO</h1>
    <button @click="fetchTodo">Fetch Data</button>
    <p>Completed Items: {{ completedItems }}</p>
    <p>Remaining Items: {{ remainingItems }}</p>
    <ul>
      <li v-for="todo in todoData" :key="`todo-id-${todo.id}`">
        <input type="checkbox" :checked="todo.completed" /> {{ todo.title }}
      </li>
    </ul>
  </div>
</template>

<script setup>
const todoData = ref(null);

const fetchTodo = async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    const data = await response.json();
    todoData.value = data;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};
const completedItems = computed(() => {
  if (!todoData.value) return 0;
  return todoData.value.filter((todo) => todo.completed).length;
});

const remainingItems = computed(() => {
  if (!todoData.value) return 0;
  return todoData.value.filter((todo) => !todo.completed).length;
});
</script>
