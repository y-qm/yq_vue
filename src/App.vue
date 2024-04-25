<template>
  <div class="box">
    <div class="add">
      <input type="text" class="writer" v-model="writeText" />
      <button class="confirm_add" @click="handleWriter">新建</button>
    </div>
    <ListVue :list="list" v-if="list.length>0" :onUpdate="handleUpdate" :onDelete="handleDelete"/>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
}
a,
ul,
ol {
  text-decoration: none;
  list-style: none;
}
.box {
  width: 800px;
  margin: auto;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
  background-color: #fff;
  padding: 34px;
  margin-bottom: 20px;
}

button {
  cursor: pointer;
  outline: none;
}

/* 文本写入区域 */
.box .add {
  padding-bottom: 30px;
  border-bottom: 1px solid #e2e2e2;
  display: flex;
  align-items: center;
}

.box .add .confirm_add {
  padding: 4px 10px;
  background-color: #0082fc;
  color: #fff;
  opacity: 0.8;
  margin-left: 20px;
  border-radius: 0;
}

.box .add .writer {
  padding: 4px;
  outline: none;
  border-radius: 4px;
  border: 1px solid #282828;
}
.box .add .writer:focus {
  border-color: #0082fc;
}
.box .add .confirm_add:hover {
  opacity: 1;
}
</style>

<script setup>
import { reactive, ref } from 'vue';
import ListVue from './components/List.vue';
import {handleConfirm} from './utils'

// 列表数据
const list = reactive([{ message: '第一条' }]);

// 写入文本
const writeText = ref('');

// 写入处理程序
const handleWriter = () => {
  const { value } = writeText;
  if (!value) {
    alert('输入内容不能为空');
    return;
  }
  const content = {
    message: value,
  };
  list.push(content);
  writeText.value = '';
};

// 删除处理程序
const handleDelete = (index,item) =>{
  if (!handleConfirm("您确定要删除此条数据吗？")){
    return
  }
  list.splice(index,1)
}

// 修改处理程序
const handleUpdate = (index,item,message) =>{
  if (!handleConfirm("您确定要修改此条数据吗？")){
    return
  }
  
  // 当前没有与后端通信的步骤，因此直接在 item 的引用操作数据变更，触发 Vue 的响应式
  item.message = message;
}

</script>
