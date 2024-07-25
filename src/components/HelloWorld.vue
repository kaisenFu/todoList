<template>  
  <div>  
    <h1 class="title">待办事项列表</h1>  
    <div>  
      <input v-model="newTodoText" placeholder="添加新任务" />  
      <a class="addButton" @click="addTodo">添加任务</a>  
    </div>  
  
    <!-- 进行中的任务 -->  
    <div>  
      <h2>进行中 ({{ activeCount }})</h2>  
      <ul>  
        <li v-for="todo in activeTodos" :key="todo.id">  
          <input type="checkbox" :checked="todo.completed" @change="toggleTodo(todo.id)" />  
          <span @dblclick="startEditing(todo.id)">{{ todo.isEditing ? '' : todo.text }}</span>  
          <input v-if="todo.isEditing" v-model="todo.tempText" @blur="saveEditing(todo.id)" />  
          <a class="button" v-if="todo.isEditing" @click="saveEditing(todo.id)">保存</a>  
          <a class="button" v-else @click="startEditing(todo.id)">修改</a>  
          <a class="button" @click="deleteTodo(todo.id)">删除</a>  
        </li>  
      </ul>  
    </div>  
  
    <!-- 已完成的任务 -->  
    <div>  
      <h2>已完成 ({{ completedCount }})</h2>  
      <ul>  
        <li v-for="todo in completedTodos" :key="todo.id">  
          <input type="checkbox" :checked="todo.completed" disabled />  
          <span @dblclick="startEditing(todo.id)">{{ todo.isEditing ? '' : todo.text }}</span>
          <input v-if="todo.isEditing" v-model="todo.tempText" @blur="saveEditing(todo.id)" />  
          <a class="button" v-if="todo.isEditing" @click="saveEditing(todo.id)">保存</a>  
          <a class="button" v-else @click="startEditing(todo.id)">修改</a>
          <a class="button" @click="deleteTodo(todo.id)">删除</a>  
        </li>  
      </ul>  
    </div>  
  </div>  
</template> 
  
<script setup>  
import { ref, computed } from 'vue';  
  
const todos = ref([  
    { id: 1, text: '学习Vue 3', completed: false },  
    { id: 2, text: '完成todoList', completed: true }, 
]);  
  
const newTodoText = ref('');  
  
function addTodo() {  
  if (newTodoText.value.trim() !== '') {  
    todos.value.push({ id: todos.value.length + 1, text: newTodoText.value, completed: false, isEditing: false, tempText: '' });  
    newTodoText.value = '';  
  }  
}  
  
function toggleTodo(id) {  
  const todo = todos.value.find(todo => todo.id === id);  
  if (todo) {  
    todo.completed = !todo.completed;  
  }  
}  
  
function deleteTodo(id) {  
  todos.value = todos.value.filter(todo => todo.id !== id);  
}  
  
function startEditing(id) {  
  const todo = todos.value.find(todo => todo.id === id);  
  if (todo) {  
    todo.isEditing = true;  
    todo.tempText = todo.text; // 临时存储原始文本以便撤销  
  }  
}  
  
function saveEditing(id) {  
  const todo = todos.value.find(todo => todo.id === id);  
  if (todo && todo.tempText.trim() !== '') {  
    todo.text = todo.tempText;  
    todo.isEditing = false;  
    todo.tempText = ''; // 清空临时文本  
  } else if (todo) {  
    // 如果用户没有输入任何内容，则取消编辑  
    todo.isEditing = false;  
    todo.text = todo.tempText = ''; // 可能不需要清空text，这里取决于你的需求  
  }  
}  
  
const activeTodos = computed(() => todos.value.filter(todo => !todo.completed));  
const completedTodos = computed(() => todos.value.filter(todo => todo.completed));  
  
const activeCount = computed(() => activeTodos.value.length);  
const completedCount = computed(() => completedTodos.value.length);  
</script>  
  
<style scoped>  
.button{
  color:black;
  background-color: rgb(254, 251, 252);
  margin-left: 5%;
}
.addButton{
    color:black;
    background-color: rgb(254, 251, 252);
    margin-left: 2%;
}
.title{
  color: rgb(254, 254, 254);
  background-color: black;
}
</style>