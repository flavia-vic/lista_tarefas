<script setup>
import { ref, computed } from 'vue'

// Variável para gerar IDs únicos para cada tarefa
let id = 0

// Referência para o texto da nova tarefa
const newTodo = ref('')

// Referência para controlar se as tarefas concluídas devem ser ocultadas
const hideCompleted = ref(false)

// Lista de tarefas iniciais
const todos = ref([
  { id: id++, text: 'Fazer ovos mechidos', done: true },
  { id: id++, text: 'Escovar os dentes', done: true },
  { id: id++, text: 'Praticar leitura', done: false }
])

// Computed property para filtrar tarefas com base no valor de hideCompleted
const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

// Função para adicionar uma nova tarefa à lista
function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

// Função para remover uma tarefa da lista
function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <article>
    <header>
      <h2>Minha Lista de Tarefas</h2>
    </header>
    <form @submit.prevent="addTodo">
      <label for="new-todo">Adicionar Tarefa</label>
      <input id="new-todo" type="text" v-model="newTodo" placeholder="Nova Tarefa" aria-label="Text">
      <button type="submit">Adicionar Tarefa</button>
    </form>
    <ul>
      <!-- Renderiza a lista de tarefas, aplicando a filtragem baseada no computed property -->
      <li v-for="todo in filteredTodos" :key="todo.id">
        <label>
          <input type="checkbox" v-model="todo.done" class="checkbox">
          <!-- Aplica uma classe 'done' se a tarefa estiver marcada como concluída -->
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
        </label>
        <!-- Botão para remover a tarefa da lista -->
        <button @click="removeTodo(todo)" class="delete-button">X</button>
      </li>
    </ul>
    <!-- Botão para alternar a visibilidade das tarefas concluídas -->
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? 'Mostrar todas' : 'Ocultar concluídas' }}
    </button>
  </article>
</template>

<style>
body {
  background-color: #fef9ef;
}

h2 {
  color: #ff7f7f;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh; 
  padding: 0 2rem;
  background-color: #fef9ef; 
}

article {
  padding: 20px;
  border-radius: 10px;
  background-color: white;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  width: 100%;
  margin: 0 auto; 
}

input[type="text"] {
  border-radius: 10px;
  border: 1px solid #ffb6c1;
  outline: none;
  padding: 8px;
  width: calc(100% - 16px); 
}

button {
  border-radius: 10px;
  background-color: #ffb6c1;
  border: none;
  color: white;
  cursor: pointer;
  padding: 8px 12px;
  margin-top: 10px;
}

button:hover {
  background-color: #ff7f7f;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 20px 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 10px;
  background-color: #fff0f5;
}

span.done {
  text-decoration: line-through;
  color: #ff7f7f;
}

label {
  color: #ff7f7f;
}

.checkbox {
  margin-right: 10px;
}

.delete-button {
  background-color: transparent;
  border: none;
  color: #ff7f7f;
  font-size: 1em;
  cursor: pointer;
}

.delete-button:hover {
  color: #ff0000;
}
</style>
