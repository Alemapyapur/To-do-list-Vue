<template>
  <div class="body">
    <HeaderVue />

    <section class="container">
      <form @submit.prevent="addTodoToList" class="card">
        <div class="todo-top">
          <input
            type="text"
            name="todo"
            id="todo"
            v-model="todoText"
            class="todo-input"
          />
          <button class="todo-button">
            <span v-if="currentEditIndex == -1" class="agregar">Agregar</span>
            <span v-else class="guardar">Guardar</span>
          </button>
          <button @click="check">Marcar Todo</button>
        </div>
      </form>
      <div class="todo-lista">
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
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                width="16"
                height="16"
              >
                <path fill="none" d="M0 0h24v24H0z" />
                <path
                  d="M21 6.757l-2 2V4h-9v5H5v11h14v-2.757l2-2v5.765a.993.993 0 0 1-.993.992H3.993A1 1 0 0 1 3 20.993V8l6.003-6h10.995C20.55 2 21 2.455 21 2.992v3.765zm.778 2.05l1.414 1.415L15.414 18l-1.416-.002.002-1.412 7.778-7.778z"
                  fill="rgba(36,195,61,1)"
                />
              </svg>
            </button>
            <button class="eliminar" @click="removeTodo(index)">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                width="16"
                height="16"
              >
                <path fill="none" d="M0 0h24v24H0z" />
                <path
                  d="M17 4h5v2h-2v15a1 1 0 0 1-1 1H5a1 1 0 0 1-1-1V6H2V4h5V2h10v2zM9 9v8h2V9H9zm4 0v8h2V9h-2z"
                  fill="rgba(255,0,0,1)"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
      <p class="text-empty" v-if="todos.length === 0">
        La Lista de Tareas esta vacía.
      </p>
    </section>

    <FooterVue />
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
      todos.done = !toDo.done // buleanos (true or false, of done)
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

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.card {
  display: flex;
  flex-direction: column;
  background: blueviolet;
  border-radius: 10px;
  margin: 20px;
  padding: 10px;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 300px;
}

.left {
  display: flex;
  align-items: center;
}
.todo-input {
  border: none;
  background: whitesmoke;
}

.isDone {
  text-decoration: line-through;
  text-decoration-color: red;
}

.checkleft {
  margin-right: 10px;
}

.eliminar,
.editar {
  border: none;
  background: none;
}

.text-empty {
  text-align: center;
}
</style>
