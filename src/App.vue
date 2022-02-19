<template>
  <div class="main">
    <HeaderVue class="title" />

      <form @submit.prevent="addTodoToList" class="form">
        <div class="todo-top">
          <input
            type="text"
            name="todo"
            id="todo"
            placeholder="Escribe tu tarea aquí"
            v-model="todoText"
            class="todo-input"
          />
          <button class="todo-button">
            <span v-if="currentEditIndex == -1" class="agregar">Agregar</span>
            <span v-else class="guardar">Guardar</span>
          </button>
          <button @click="check" class="todo-button">Marcar Todo</button>
        </div>
      </form>
      <div class="tasks">
        <div v-for="(todo, index) in todos" class="todo-item" :key="todo.id">
          {{ task.name }}
          <div class="left">
            <input type="checkbox" v-model="todo.isDone" class="checkleft" />
            <p :class="{ isDone: todo.isDone }" @click="addText(todo)">
              {{ todo.text }}
            </p>
          </div>

          <div class="right">
            <button class="editar" @click="editTodo(index)">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="14" height="14"><path fill="none" d="M0 0h24v24H0z"/><path d="M9.243 19H21v2H3v-4.243l9.9-9.9 4.242 4.244L9.242 19zm5.07-13.556l2.122-2.122a1 1 0 0 1 1.414 0l2.829 2.829a1 1 0 0 1 0 1.414l-2.122 2.121-4.242-4.242z" fill="rgba(255,255,255,1)"/></svg>
            </button>
            <button class="eliminar" @click="removeTodo(index)">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="14" height="14"><path fill="none" d="M0 0h24v24H0z"/><path d="M6.535 3H21a1 1 0 0 1 1 1v16a1 1 0 0 1-1 1H6.535a1 1 0 0 1-.832-.445l-5.333-8a1 1 0 0 1 0-1.11l5.333-8A1 1 0 0 1 6.535 3zM13 10.586l-2.828-2.829-1.415 1.415L11.586 12l-2.829 2.828 1.415 1.415L13 13.414l2.828 2.829 1.415-1.415L14.414 12l2.829-2.828-1.415-1.415L13 10.586z" fill="rgba(255,255,255,1)"/></svg>
            </button>
          </div>
        </div>
        <p class="text-empty" v-if="todos.length === 0">
          La Lista de Tareas esta vacía.
        </p>
      </div>

    <FooterVue class="footer" />
  </div>
</template>

<script>
import HeaderVue from './components/Header.vue'
import FooterVue from './components/Footer.vue'
import { ref } from '@vue/reactivity'

export default {
  name: 'App',
  components: {
    HeaderVue,
    FooterVue
  },
  data () {
    return {
      task: [
        { name: 'Hoy es un buen dia' },
        { name: 'Aprendamos mas que ayer' }
      ]
    }
  },
  setup () {
    let todoText = ref()
    let todos = ref([])
    let currentEditIndex = ref(-1)

    let editTodo = index => {
      currentEditIndex.value = index
      todoText.value = todos.value[index].text
    }

    let addTodoToList = () => {
      if (todoText.value) {
        if (currentEditIndex.value == -1) {
          todos.value = [
            ...todos.value,
            { text: todoText.value, isDone: false }
          ]
        } else {
          let todoInQ = todos.value[currentEditIndex.value]

          todos.value[currentEditIndex.value] = {
            text: todoText.value,
            isDone: todoInQ.isDone
          }
        }
        currentEditIndex.value = -1
        todoText.value = ''
      }
    }

    let removeTodo = index => {
      todos.value.splice(index, 1)
    }

    function check () {
      todos.value.forEach(todo => (todo.isDone = true))
    }

    function addText (toDo) {
      toDo.isDone = !toDo.isDone // buleanos (true or false, of done)
    }
    return {
      todoText,
      todos,
      currentEditIndex,
      editTodo,
      addTodoToList,
      removeTodo,
      check,
      addText
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

.main {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 10vh 20vh 60vh 10vh;
  gap: 0px 0px;
  grid-auto-flow: row;
  justify-items: center;
  align-items: center;
  justify-content: center;
  grid-template-areas:
    'tittle'
    'form'
    'tasks'
    'footer';
  background: #334d50; /* fallback for old browsers */
  background: linear-gradient( to right, #cbcaa5, #334d50 );
}

.tittle {
  grid-area: tittle;
}

.form {
  grid-area: form;
  display: flex;
  justify-content: center;
  width: 100%;
}

.tasks {
  grid-area: tasks;
  overflow-x: overlay;
  overflow-y: overlay;
  height: -webkit-fill-available;
  width: 80%;
}

.tasks button:hover {
  transform: scale(1.2);
}

.tasks::-webkit-scrollbar {
  width: 10px;
  height: 5px;
}

.tasks::-webkit-scrollbar-thumb {
  background: gray;
  border-radius: 10px;
}

.tasks::-webkit-scrollbar {
  background: rgba(128, 128, 128, 0);
}

.footer {
  grid-area: footer;
}

.todo-button {
  font-size: 12px;
  width: 100px;
  margin: 10px;
  font-family: Arial;
  border-width: 1px;
  padding: 10px;
  color: #030303;
  border-color: #dcdcdc;
  font-weight: bold;
  border-radius: 6px;
  box-shadow: inset 0px 1px 0px 0px #ffffff;
  background: linear-gradient(#818181, #f3e6e6);
}

.todo-button:hover {
  background: linear-gradient(#f6f6f6, #ffffff);
}

.left {
  display: flex;
  align-items: center;
}

.todo-input {
  border: none;
  background: whitesmoke;
  height: 37px;
  border-radius: 4px;
  width: 300px;
  padding: 10px;
  margin-right: 10px;
}

.isDone {
  text-decoration: line-through;
  text-decoration-color: red;
}

.checkleft {
  margin-right: 10px;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  background-color: rgb(240, 245, 245);
  box-shadow: 23px 23px 6px -4px rgba(0, 0, 0, 0.1);
  padding: 5px 10px 5px 10px;
  margin: 4px;
  border-radius: 4px;
}
.eliminar,
.editar {
  border: none;
  background: black;
  padding: 4px;
  border-radius: 3px;
  margin: 4px;
}

.text-empty {
  text-align: center;
}
</style>
