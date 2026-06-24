<template>
    <div class="module-box">
      <h3 class="module-text">待办事项</h3>
      <div class="input-group">
        <input
        v-model="localTodo"
        placeholder="输入待办事项"
        class="input-item">
        <button @click="handleAddTodo" class="btn add-btn">添加</button>
      </div>
      <!--待办列表-->
      <div v-if="todoList.length"  class="list-wrap">
        <div v-for="(item ,index) in todoList" :key="index"  class="list-item">
          <!--修改删除-->
          <!--<span>{{ index +1 }} . {{ item }}</span>-->
          <span 
            v-bind:class="{ finished: item.done }" 
            @click="handleToggleDone(index)"
          >
            {{ index +1 }} . {{ item.text }}
          </span>
          <button @click="handleDelTodo(index )" class="btn del-btn">删除</button>
        </div>
      </div>
      <p v-else  class="empty-tip">暂无待办事项</p>
    </div>
    
</template>


<script setup>
import { ref } from 'vue'
// 接收父传待办数组
const props = defineProps({
  todoList: {
    type: Array,
    required: true,
    default: () => []
  }
})
// 定义抛出事件
const emit = defineEmits(['add-todo','toggle-done','del-todo'])

// 本地输入框
const localTodo = ref('')

// 添加待办，把输入内容传给父
const handleAddTodo = () => {
  emit('add-todo', localTodo.value)
  localTodo.value = ''
}
// 切换完成状态，抛下标
const handleToggleDone = (index) => {
  emit('toggle-done', index)
}
// 删除待办，抛下标
const handleDelTodo = (index) => {
  emit('del-todo', index)
}
</script>
<style scoped>

/* 复用 Score.vue 的样式，保持统一（可抽离公共样式，这里简化） */
.module-box {
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 25px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.08);
}
.module-title {
  margin-top: 0;
  color: #333;
  border-left: 4px solid #409eff;
  padding-left: 10px;
}
.input-group {
  display: flex;
  gap: 10px;
  margin: 15px 0;
}
.input-item {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}
.input-item:focus {
  border-color: #409eff;
}
.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: opacity 0.2s;
}
.btn:hover {
  opacity: 0.9;
}
.add-btn {
  background-color: #409eff;
  color: #fff;
}
.del-btn {
  background-color: #f56c6c;
  color: #fff;
}
.list-wrap {
  margin: 15px 0;
}
.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 12px;
  background-color: #f9fafb;
  border-radius: 4px;
  margin-bottom: 8px;
}
.empty-tip {
  color: #999;
  text-align: center;
  margin: 10px 0;
}
.finished {
  text-decoration: line-through;
  color: #999;
  cursor: pointer;
}
</style>
