<template>
  <div id="app">
    <div v-html="content" class="title-box"></div>
    <h1>小猴待办事件</h1>
    <!--成绩模块-->
     <Score 
      :scoreList="scoreList" 
      @add-score="handleAddScore" 
      @del-score="handleDelScore"
      :currentScore="score"
    />
    
    <!--成绩评价-->
    <div class="comment-box">
       <!-- 判断列表有没有数据，拿最后一条成绩做评价 -->
      <p v-if="scoreList.length && Number(scoreList.at(-1)) >=85" class="text-good">成绩优异,请你继续保持</p>
      <p v-else-if="scoreList.length && Number(scoreList.at(-1)) >=60" class="text-normal">成绩合格，请你再接再厉</p>
      <p v-else-if="scoreList.length && Number(scoreList.at(-1)) <60" class="text-bad">成绩不合格，你需要努力学习</p>
      <p v-else class="text-no"> 无</p>
    </div>
    <div class="quote-box">
      <h2 v-bind:title="tip" style="color: brown;">天子以自强不息，君子以厚德载物</h2>
    </div>
    <!--待办事项模块-->
    <Todo
    :todoList="todoList"
    @add-todo="handleAddTodo"
    @toggle-done="handleToggleDone"
    @del-todo="handleDelTodo"
    />
    
  </div>
</template>  



<script setup>
import { handleError, ref } from 'vue';
import Todo from './components/Todo.vue';
import Score from './components/Score.vue';
const content=ref('<span style="color:red;">！！！</span>')
const tip=ref('这个是一个彩蛋')
// 1. 接收用户输入的成绩
const score = ref('0')
// 2. 存放所有成绩的列表
const scoreList = ref([])

// 添加成绩
  // 简单判断：非空才添加
const handleAddScore = (newScore) => {
  // 统一转成字符串，再去除空格
const str = newScore == null ? '' : String(newScore)
const val = str.trim()
  // 不为空才添加
  if (val !== '') {
    scoreList.value.push(val)
    score.value = ''
  }
}

// 删除指定成绩（根据下标删除）
const handleDelScore = (index) => {
  scoreList.value.splice(index, 1)
}

// 接收用户输入的待办内容
const todo = ref('')
// 存储所有待办事项的数组
const todoList = ref([])

// 添加待办
const handleAddTodo = (todoText) => {
  // 去除首尾空格，非空才添加
  const content = todoText.trim()
  if (content) {
    todoList.value.push({
      text:content,
      done:false
    })
    // 添加后清空输入框
    todo.value = ''
  }
}

// 切换待办完成状态
const handleToggleDone = (index) => {
  todoList.value[index].done = !todoList.value[index].done
}

// 删除单条待办，根据下标删除
const handleDelTodo = (index) => {
  todoList.value.splice(index, 1)
}
</script>

<style scoped>
/* 全局容器 */
#app {
  max-width: 600px;
  margin: 30px auto;
  padding: 0 20px;
  font-family: "Microsoft Yahei", sans-serif;
}

/* 顶部标题 */
.title-box {
  text-align: center;
  margin-bottom: 30px;
}
/* 成绩评价文字 */
.comment-box p {
  padding: 8px 12px;
  border-radius: 4px;
  margin: 10px 0 0;
}
.text-good {
  background-color: #f0f9ff;
  color: #67c23a;
}
.text-normal {
  background-color: #fffbeb;
  color: #e6a23c;
}
.text-bad {
  background-color: #fef0f0;
  color: #f56c6c; 
}
.text-no {
  background-color: #fef0f0;
  color: #f56c6c;
}
/* 名言区域 */
.quote-box {
  text-align: center;
  margin: 25px 0;
  color: #8b4513;
}
.quote-box h2 {
  font-size: 18px;
  font-weight: normal;
}
/* 【新增】已完成样式：删除线 + 灰色文字 */
.finished {
  text-decoration: line-through;
  color: #999;
  cursor: pointer;
}
</style>