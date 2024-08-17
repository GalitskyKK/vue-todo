<template>
  <div class="todo-list">
    <div class="input-container">
      <input
        type="text"
        v-model="searchQuery"
        @input="filterTodos"
        placeholder="Search tasks..."
        class="input search-input"
      />
      <input
        type="text"
        v-model="newTodo"
        @keyup.enter="addTodo"
        placeholder="Add a new task"
        class="input new-task-input"
      />
      <div class="tag-input">
        <input
          type="text"
          v-model="newTag"
          @keyup.enter="addTag"
          placeholder="Add tag (press Enter)"
          class="input"
        />
        <div v-if="tags.length" class="tags">
          <span
            v-for="(tag, index) in tags"
            :key="index"
            class="tag"
            @click="removeTag(index)"
          >{{ tag }}</span>
        </div>
      </div>
    </div>
    <div class="tasks-container">
      <ul>
        <li v-for="(todo, index) in filteredTodos" :key="index" class="task-item">
          <input
            type="checkbox"
            v-model="todo.completed"
            @change="saveTodos"
          />
          <span
            v-if="!todo.isEditing"
            :class="{ completed: todo.completed }"
            @dblclick="editTodo(todo)"
          >
            {{ todo.text }}
          </span>
          <input
            v-else
            type="text"
            v-model="todo.text"
            @keyup.enter="stopEditing(todo)"
            @blur="stopEditing(todo)"
            class="edit-input input"
          />
          <div v-if="todo.tags && todo.tags.length" class="todo-tags">
            <span v-for="(tag, index) in todo.tags" :key="index" class="todo-tag">{{ tag }}</span>
          </div>
          <button @click="removeTodo(index)" class="remove-button">X</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, watchEffect, onMounted } from 'vue';

export default {
  setup() {
    const newTodo = ref('');
    const searchQuery = ref('');
    const newTag = ref('');
    const tags = ref([]);
    const todos = ref(JSON.parse(localStorage.getItem('todos')) || []);
    const filteredTodos = ref([]);

    const addTodo = () => {
      if (newTodo.value.trim() !== '') {
        todos.value.push({
          text: newTodo.value,
          completed: false,
          tags: [...tags.value],
          isEditing: false,
        });
        newTodo.value = '';
        tags.value = [];
        saveTodos();
        filterTodos();
      }
    };

    const removeTodo = (index) => {
      todos.value.splice(index, 1);
      saveTodos();
      filterTodos();
    };

    const addTag = () => {
      if (newTag.value.trim() !== '' && !tags.value.includes(newTag.value)) {
        tags.value.push(newTag.value);
        newTag.value = '';
      }
    };

    const removeTag = (index) => {
      tags.value.splice(index, 1);
    };

    const filterTodos = () => {
      const query = searchQuery.value.toLowerCase();
      filteredTodos.value = todos.value.filter(todo =>
        todo.text.toLowerCase().includes(query) ||
        (todo.tags && todo.tags.some(tag => tag.toLowerCase().includes(query)))
      );
    };

    const saveTodos = () => {
      localStorage.setItem('todos', JSON.stringify(todos.value));
    };

    const editTodo = (todo) => {
      todo.isEditing = true;
    };

    const stopEditing = (todo) => {
      if (todo.text.trim() === '') {
        removeTodo(todos.value.indexOf(todo));
      } else {
        todo.isEditing = false;
        saveTodos();
      }
    };

    watchEffect(filterTodos);

    onMounted(() => {
      filterTodos();
    });

    return {
      newTodo,
      searchQuery,
      newTag,
      tags,
      todos,
      filteredTodos,
      addTodo,
      removeTodo,
      addTag,
      removeTag,
      filterTodos,
      saveTodos,
      editTodo,
      stopEditing,
    };
  },
};
</script>

<style scoped>
.todo-list {
  width: 100%;
  display: flex;
  flex-direction: column;
}

.input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.input {
  width: 100%;
  padding: 12px;
  margin-bottom: 10px;
  border: 1px solid #3333332d;
  border-radius: 10px;
  background: #ffffff;
  color: #000000;
  box-shadow: inset 0px 1px 2px rgba(0, 0, 0, 0.1);
  transition: all 0.15s ease-in-out;
}

.input:focus {
  outline: none;
  border: 1px solid #33333375;
}

.search-input {
  margin-bottom: 30px;
}

.new-task-input {
  margin-top: 20px;
}

.tag-input {
  margin-bottom: 20px;
}

.tasks-container {
  margin-top: 20px;
  background: #ffffff;
  color: #2b2b2b;
  font-weight: 600;
  padding: 20px;
  border-radius: 10px;
  box-shadow: inset 0px 1px 5px rgba(0, 0, 0, 0.1);
}

ul {
  list-style: none;
  padding: 0;
}

.task-item {
  display: flex;
  gap: 10px;
  align-items: center;
  padding: 12px;
  margin-bottom: 10px;
  background: #ffffff;
  border: 1px solid #3333331e;
  border-radius: 10px;
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.1);
}

.task-item .completed {
  text-decoration: line-through;
  color: #777;
}

.edit-input {
  margin-right: 10px;
}

.todo-tags {
  margin-left: 10px;
  display: flex;
  flex-wrap: wrap;
}

.todo-tag {
  background-color: #eee;
  color: #000;
  padding: 3px 8px;
  border-radius: 12px;
  margin-right: 5px;
}

.tag {
  background-color: #eee;
  color: #000;
  padding: 3px 8px;
  border-radius: 12px;
  margin-right: 5px;
}

.remove-button {
  margin-left: auto;
  height: 20px;
  width: 20px;
  border: none;
  background: #e74c3c;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
}

.remove-button:hover {
  background: #c0392b;
}
</style>
