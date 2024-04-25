<template>
  <ul class="list">
    <li
      class="item"
      v-for="(item, index) in props.list"
      :key="`${index}_${item.message}`"
    >
      <template v-if="selectedIndex !== index">
        <div class="message">{{ item.message }}</div>
        <div class="option">
          <button class="delete btn" @click="props.onDelete(index, item)">
            删除
          </button>
          <button class="update btn" @click="handleUpdateIndex(index,item.message)">
            修改
          </button>
        </div>
      </template>
      <template v-else>
        <input type="text" class="write_update_info" v-model="updateText"/>
        <div class="option">
            <button class="delete btn" @click="props.onDelete(index, item)">
          删除
        </button>
        <button class="update btn" @click="onSave(index, item)">保存</button>
        </div>
      </template>
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue';
const props = defineProps({
  list: {
    type: Array,
    required: true,
  },
  onUpdate: {
    type: Function,
    required: true,
  },
  onDelete: {
    type: Function,
    required: true,
  },
});

// 记录当前修改 项 的索引
const selectedIndex = ref(-1);
const handleUpdateIndex = (index,message) => {
  selectedIndex.value = index;
  updateText.value = message;
};

// 记录修改文本内容
const updateText = ref('');

// 在确认修改后重置旧文本内容
const onSave = (index, item) => {
  props.onUpdate(index, item, updateText.value);
  updateText.value = '';
  selectedIndex.value = -1
};
</script>

<style scoped>
/* 列表区域 */
.list .item {
  box-sizing: border-box;
  padding: 20px;
  border-bottom: 1px solid #e2e2e2;
  text-align-last: left;
}
.list .item .option {
  margin-top: 16px;
  display: flex;
  align-items: center;
}
.list .item .option .btn {
  border-radius: 0;
  font-size: 14px;
  opacity: 0.8;
  padding: 4px 8px;
  background-color: #fff;
}
.list .item .option .btn:hover {
  opacity: 1;
}

.list .item .option .delete {
  margin-right: 20px;
  border: 1px solid #fc0000;
  color: #fc0000;
}

.list .item .option .delete:hover {
  background-color: #fc0000;
  color: #fff;
}

.list .item .option .update {
  border: 1px solid #0082fc;
  color: #0082fc;
}
.list .item .option .update:hover {
  background-color: #0082fc;
  color: #fff;
}

</style>
