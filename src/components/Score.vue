<template>
  
<div class="module-box">
      <h3 class="module-title">成绩录入</h3>
      <div class="input-group">
        <input
        v-model="localScore"
        type="number"
        placeholder="输入你这次的成绩"
        class="input-item">
        <button @click="handleAdd" class="btn add-btn">添加</button>
      </div>
      <!--成绩列表-->
      <div>
        <div v-if="scoreList.length > 0" class="list-wrap">
          <h4>成绩列表</h4>
          <div v-for="(item,index) in scoreList" :key="index" class="list-item">
            成绩{{  item }}
            <button @click="handleDel(index)" class="btn del-btn">删除</button>
          </div>
        </div>
        <p v-else class="empty-tip">暂无成绩获取</p>
      </div>
    </div>
</template>

<script setup>
import { ref, watch } from 'vue';
// 接收父组件传过来的数据
const props = defineProps({
  scoreList: {
    type: Array,
    required: true,
    default: () => []
  },
  currentScore: {
    type: String,
    default: '0'
  }
})
// 声明要发送给父组件的事件
const emit = defineEmits(['add-score', 'del-score'])

// 本地输入框临时值，不直接修改props
const localScore = ref(props.currentScore)

// 监听父组件清空输入框，同步子组件
watch(() => props.currentScore, (val) => {
  localScore.value = val
})

// 添加按钮：抛事件给父
const handleAdd = () => {
  emit('add-score', localScore.value)
}
// 删除按钮：抛下标给父
const handleDel = (index) => {
  emit('del-score', index)
}
</script>

<style scoped>
/* 模块容器 */
.module-box {
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 25px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.08);
}

/* 模块标题 */
.module-title {
  margin-top: 0;
  color: #333;
  border-left: 4px solid #409eff;
  padding-left: 10px;
}

/* 输入行布局 */
.input-group {
  display: flex;
  gap: 10px;
  margin: 15px 0;
}

/* 输入框样式 */
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

/* 按钮通用样式 */
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

/* 添加按钮 */
.add-btn {
  background-color: #409eff;
  color: #fff;
}

/* 删除按钮 */
.del-btn {
  background-color: #f56c6c;
  color: #fff;
}

/* 列表容器 */
.list-wrap {
  margin: 15px 0;
}

/* 列表每一项 */
.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 12px;
  background-color: #f9fafb;
  border-radius: 4px;
  margin-bottom: 8px;
}

/* 空数据提示 */
.empty-tip {
  color: #999;
  text-align: center;
  margin: 10px 0;
}

</style>