<script setup>
const task = ref('');
const { data: tasks, refresh } = useFetch('/api/task');

const addTask = async () => {
  const response = await fetch('/api/task', {
    method: 'post',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ task: task.value }),
  });

  if (response.ok) {
    const newTask = await response.json();
    tasks.value.push(newTask);
    task.value = '';
  } else {
    console.error('Error adding task:', response.status, response.statusText);
  }
};

const deleteTask = async (id) => {
  const response = await fetch(`/api/task?id=${id}`, {
    method: 'DELETE',
  });

  if (response.ok) {
    tasks.value = tasks.value.filter((task) => task.id !== id);
  } else {
    console.error('Error deleting task:', response.status, response.statusText);
  }
};
</script>
<template>
  <div>
    <h1>Main Page</h1>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        {{ task.task }}
        <button @click="deleteTask(task.id)">削除</button>
      </li>
    </ul>
    <form @submit.prevent="addTask">
      <div>
        <input v-model="task" />
      </div>
      <div>
        <button type="submit">タスクを登録</button>
      </div>
    </form>
  </div>
</template>
