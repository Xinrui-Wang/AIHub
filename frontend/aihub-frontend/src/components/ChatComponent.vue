<template>
  <div class="chat-container">
    <!-- 引入并使用 ButtonBar 组件 -->
    <ButtonBar
      @button2-click="handleButton2"
      @button3-click="handleButton3"
      @model-changed="handleModelChange"
    />

    <!-- 消息列表 -->
    <div ref="messagesList" class="messages-list">
      <div
        v-for="(message, index) in messages"
        :key="index"
        class="message-box"
        :class="message.sender"
      >
        {{ message.text }}
      </div>
    </div>

    <!-- 输入框容器 -->
    <div class="input-container">
      <textarea
        v-model="inputMessage"
        class="message-input"
        placeholder="Type your message..."
        @input="adjustTextareaHeight"
      ></textarea>
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script>
import ButtonBar from './ChatComponentTopBar.vue'; // 引入 ButtonBar 组件

export default {
  components: {
    ButtonBar, // 注册 ButtonBar 组件
  },
  data() {
    return {
      messages: [],
      inputMessage: "",
      selectedModel: "模型 A", // 默认模型
    };
  },
  methods: {
    // 处理模型切换
    handleModelChange(model) {
      this.selectedModel = model;
      console.log("切换到的模型:", model); // 你可以根据这个模型来进行后续的处理，如请求不同的 API
    },
    async sendMessage() {
      if (this.inputMessage.trim() !== "") {
        const userText = this.inputMessage.trim();
        this.messages.push({ text: userText, sender: "user" });
        this.inputMessage = "";
        this.$nextTick(() => {
          const textarea = document.querySelector(".message-input");
          textarea.style.height = "40px";
        });
        this.scrollToBottom();
      }
    },
    scrollToBottom() {
      this.$nextTick(() => {
        const chatContainer = this.$refs.messagesList;
        if (chatContainer) {
          chatContainer.scrollTop = chatContainer.scrollHeight;
        }
      });
    },
    adjustTextareaHeight(event) {
      const textarea = event.target;
      textarea.style.height = "auto";
      textarea.style.height = textarea.scrollHeight + "px";
    },
  },
};
</script>

<style scoped>
/* ChatComponent 样式保持不变 */
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  overflow: hidden;
}

.messages-list {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
  background-color: #f6f5f9;
}

.input-container {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #ffffff;
  border-top: 1px solid #ddd;
}

.message-input {
  flex: 1;
  min-height: 40px;
  max-height: 150px;
  resize: none;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 5px;
  box-sizing: border-box;
  overflow-y: auto;
  font-size: 14px;
}

button {
  margin-left: 10px;
  padding: 8px 15px;
  background-color: #007bff;
  color: #ffffff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.message-box {
  margin: 5px 0;
  padding: 10px;
  border-radius: 10px;
  max-width: 70%;
  word-wrap: break-word;
}

.message-box.user {
  align-self: flex-end;
  background-color: #d1e7dd;
}

.message-box.system {
  align-self: center;
  background-color: #f8d7da;
}
</style>
