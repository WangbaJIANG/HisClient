<template>
  <div>
    <!-- AI助手按钮 -->
    <button class="chat-button" @click="toggleChat">AI 助手</button>

    <!-- 聊天窗口 -->
    <div v-if="showChat" class="chat-box">
      <div class="chat-header">
        <span>🤖 AI 助手</span>
        <button @click="toggleChat">✖</button>
      </div>
      <div class="chat-messages" ref="chatMessages">
        <div v-for="(msg, i) in messages" :key="i" :class="msg.role">
          <strong>{{ msg.role === 'user' ? '你' : 'AI' }}：</strong> {{ msg.content }}
        </div>
      </div>
      <div class="chat-input">
        <input v-model="input" @keyup.enter="sendMessage" placeholder="输入消息..." />
        <button @click="sendMessage">发送</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      showChat: false,
      input: '',
      messages: []
    }
  },
  methods: {
    toggleChat() {
      this.showChat = !this.showChat
    },
    async sendMessage() {
      if (!this.input.trim()) return
      const userMsg = { role: 'user', content: this.input }
      this.messages.push(userMsg)
      const history = [...this.messages]

      this.input = ''

      try {
        const res = await axios.post('https://api.34ku.com/v1/chat/completions', {
          model: 'gpt-3.5-turbo',
          messages: history,
          temperature: 0.7
        }, {
          headers: {
            Authorization: `sk-ZcrJUl5YEDhR4xFvC6FcD7D1Aa8044Dd8f99Bc8c402f0c8d`,
            'Content-Type': 'application/json'
          }
        })
console.log('AI回复:', res.data)
        const aiReply = res.data.choices[0].message
        this.messages.push(aiReply)
        this.$nextTick(() => {
          this.$refs.chatMessages.scrollTop = this.$refs.chatMessages.scrollHeight
        })
      } catch (err) {
        console.error('请求出错:', err)
        this.messages.push({ role: 'assistant', content: '⚠️ 请求失败，请检查 API Key 或网络' })
      }
    }
  }
}
</script>

<style scoped>
.chat-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #4CAF50;
  color: white;
  padding: 12px 18px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  z-index: 1000;
}
.chat-box {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 320px;
  height: 420px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 0 10px #ccc;
  display: flex;
  flex-direction: column;
  z-index: 1000;
}
.chat-header {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  font-weight: bold;
  background: #f0f0f0;
  border-bottom: 1px solid #ddd;
}
.chat-messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  font-size: 14px;
}
.user {
  text-align: right;
  margin: 5px 0;
}
.assistant {
  text-align: left;
  margin: 5px 0;
}
.chat-input {
  display: flex;
  border-top: 1px solid #ddd;
}
.chat-input input {
  flex: 1;
  padding: 10px;
  border: none;
}
.chat-input button {
  background: #4CAF50;
  color: white;
  border: none;
  padding: 0 16px;
  cursor: pointer;
}
</style>
