<template>
    <div class="button-container">
      <!-- 下拉按钮 -->
      <div class="dropdown">
        <button class="dropdown-btn" @click="toggleDropdown">
          {{ selectedModel }} <span class="arrow">&#9662;</span>
        </button>
        <div v-if="dropdownVisible" class="dropdown-list">
          <div
            v-for="(model, index) in models"
            :key="index"
            class="dropdown-item"
            @click="selectModel(model)"
          >
            {{ model }}
          </div>
        </div>
      </div>
  
      <!-- 其他按钮 -->
      <button @click="handleButton2">按钮 2</button>
      <button @click="handleButton3">按钮 3</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        models: ['模型 A', '模型 B', '模型 C'], // 模型列表
        selectedModel: '模型 A', // 默认选中的模型
        dropdownVisible: false, // 控制下拉菜单的显示和隐藏
      };
    },
    methods: {
      // 切换下拉菜单的显示和隐藏
      toggleDropdown() {
        this.dropdownVisible = !this.dropdownVisible;
      },
      // 选择模型
      selectModel(model) {
        this.selectedModel = model;
        this.dropdownVisible = false; // 选择后关闭下拉菜单
        this.$emit('model-changed', model); // 通知父组件，触发自定义事件
      },
      handleButton2() {
        this.$emit('button2-click');
      },
      handleButton3() {
        this.$emit('button3-click');
      },
    },
  };
  </script>
  
  <style scoped>
  .button-container {
    display: flex;
    justify-content: space-around;
    padding: 10px;
    background-color: #f1f1f1;
    border-bottom: 1px solid #ddd;
  }
  
  .dropdown {
    position: relative;
  }
  
  .dropdown-btn {
    padding: 10px 20px;
    background-color: #007bff;
    border: 1px solid #ddd;
    border-radius: 5px;
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .arrow {
    margin-left: 5px;
  }
  
  .dropdown-list {
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    background-color: #ffffff;
    border: 1px solid #ddd;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
    z-index: 10;
  }
  
  .dropdown-item {
    padding: 10px 20px;
    cursor: pointer;
  }
  
  .dropdown-item:hover {
    background-color: #007bff;
    color: white;
  }
  
  button {
    padding: 10px 20px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  </style>
  