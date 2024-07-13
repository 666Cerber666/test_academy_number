<template>
  <div class="border p-4 rounded shadow-md mt-4 text-white">
    <transition-group name="fade" tag="div">
      <div v-for="(message, index) in messages" :key="index" class="mb-2 clear-both">
        <p v-if="message.from === 'bot'" class="text-black text-left bg-gray-200 p-2 rounded w-2/3 mb-2">{{ message.text }}</p>
        <p v-else class="text-black text-right bg-blue-200 p-2 rounded w-2/3 float-right mb-2">{{ message.text }}</p>
      </div>
    </transition-group>
    <input v-model="currentInput" @keyup.enter="sendMessage" class="border p-2 w-full rounded mt-2 shadow-md text-black" placeholder="Write a message">
  </div>
</template>

<script>
export default {
  data() {
    return {
      messages: JSON.parse(localStorage.getItem('chatMessages')) || [],
      currentInput: '',
      questions: [
        'Ты кто?',
        'И для чего же ты сюда пришел?',
        'Нашёл то, что искал?'
      ],
      currentQuestionIndex: 0
    };
  },
  created() {
    if (this.messages.length === 0) {
      this.messages.push({ from: 'bot', text: this.questions[this.currentQuestionIndex] });
    }
  },
  methods: {
    sendMessage() {
      if (this.currentInput.trim() === '') return;

      this.messages.push({ from: 'user', text: this.currentInput });
      localStorage.setItem('chatMessages', JSON.stringify(this.messages));

      this.currentInput = '';

      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex++;
        setTimeout(() => {
          this.messages.push({ from: 'bot', text: this.questions[this.currentQuestionIndex] });
          localStorage.setItem('chatMessages', JSON.stringify(this.messages)); // Save bot message
        }, 500);
      }
    }
  }
};
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.clear-both::after {
  content: "";
  display: block;
  clear: both;
}
</style>
