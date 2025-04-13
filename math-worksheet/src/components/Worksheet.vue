<template>
  <div class="max-w-4xl mx-auto p-5 bg-gray-50 rounded-xl shadow-lg">
    <h1 class="text-3xl font-bold text-center text-gray-800 mb-5">Rounding Off to Nearest 10</h1>
    
    <div class="flex flex-col sm:flex-row justify-between items-center bg-blue-50 p-4 rounded-lg mb-5">
      <div class="w-full sm:w-auto mb-3 sm:mb-0">
        <label for="studentName" class="block text-sm font-medium text-gray-700 mb-1">Name:</label>
        <input
          type="text"
          id="studentName"
          v-model="studentName"
          placeholder="Enter your name"
          @input="handleNameInput"
          :class="['w-full p-2 border rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500', 
                  showNameError ? 'border-red-500 animate-shake' : 'border-gray-300']"
        >
      </div>
      <div v-if="showResults" class="text-lg font-bold text-gray-800">
        Score: {{ score }}/12
      </div>
    </div>

    <p class="text-center italic text-gray-600 mb-6">Circle the correct answers.</p>


    <div class="grid grid-cols-1 lg:grid-cols-2 gap-4 mb-8">
      <div
        v-for="(question, index) in questions"
        :key="index"
        class="question-item p-4 bg-white rounded-lg shadow-sm transition-all duration-300 hover:shadow-md"
        :class="{
          'border-l-4 border-green-500 bg-green-50': showFeedback && question.selected === question.correctAnswer,
          'border-l-4 border-red-500 bg-red-50': showFeedback && question.selected && question.selected !== question.correctAnswer
        }"
      >
      <div class="font-bold text-gray-800 mb-3">{{ question.text }}</div>
        <div class="flex flex-wrap gap-2">
          <label
            v-for="(option, optIndex) in question.options"
            :key="optIndex"
            :class="[
              'flex items-center px-4 py-2 border rounded-full cursor-pointer transition-colors',
              question.selected === option ? 'bg-blue-600 text-white border-blue-600' : 'border-gray-300 hover:bg-gray-100',
              !nameEntered ? 'opacity-60 cursor-not-allowed' : ''
            ]"
          >
            <input
              type="radio"
              :name="'q' + index"
              :value="option"
              v-model="question.selected"
              @change="clearFeedback(index)"
              :disabled="!nameEntered"
              class="mr-2 cursor-pointer disabled:cursor-not-allowed"
            >
            {{ option }}
          </label>
        </div>
        <div
          v-if="showFeedback && question.selected"
          class="mt-2 font-bold animate-fade-in"
          :class="question.selected === question.correctAnswer ? 'text-green-600' : 'text-red-600'"
        >
          {{ question.selected === question.correctAnswer ? '✓ Correct!' : '✗ Incorrect' }}
        </div>      </div>
    </div>

    <div class="flex flex-col sm:flex-row justify-center gap-4 mb-8">
      <button
        @click="resetAnswers"
        :disabled="!nameEntered"
        class="px-6 py-2 bg-gray-500 text-white rounded-md transition-colors disabled:opacity-60 disabled:cursor-not-allowed hover:bg-gray-600 disabled:hover:bg-gray-500"
      >
        Reset Answers
      </button>
      <button
        @click="submitAnswers"
        :disabled="!nameEntered"
        class="px-6 py-2 bg-blue-600 text-white rounded-md transition-colors disabled:opacity-60 disabled:cursor-not-allowed hover:bg-blue-700 disabled:hover:bg-blue-600"
      >
        Submit
      </button>
    </div>

    <div
      v-if="showResults"
      class="text-center p-5 mb-6 rounded-lg animate-slide-up"
      :class="{
        'bg-green-100 text-green-800': scoreClass === 'excellent',
        'bg-blue-100 text-blue-800': scoreClass === 'good',
        'bg-yellow-100 text-yellow-800': scoreClass === 'average',
        'bg-red-100 text-red-800': scoreClass === 'poor'
      }"
    >
      <div class="font-bold text-lg">
        {{ resultMessage }}
      </div>
    </div>

    <div class="text-center text-gray-500 text-sm mt-6">
      copyright: www.mathinenglish.com
    </div>
  </div>
</template>

<script>
export default {
  name: 'WorkSheet',
  data() {
    return {
      studentName: '',
      showNameError: false,
      showFeedback: false,
      showResults: false,
      nameEntered: false,
      questions: [
        {
          text: '17 rounded off to the nearest 10 is..',
          options: ['10', '20', '17'],
          correctAnswer: '20',
          selected: null,
        },
        {
          text: '45 rounded off to the nearest 10 is..',
          options: ['50', '45', '40'],
          correctAnswer: '50',
          selected: null,
        },
        {
          text: '75 rounded off to the nearest 10 is..',
          options: ['70', '80', '175'],
          correctAnswer: '80',
          selected: null,
        },
        {
          text: '19 rounded to the nearest 10 is..',
          options: ['20', '10', '19'],
          correctAnswer: '20',
          selected: null,
        },
        {
          text: '64 rounded off to the nearest 10 is..',
          options: ['64', '70', '60'],
          correctAnswer: '60',
          selected: null,
        },
        {
          text: '0 rounded off to the nearest 10 is..',
          options: ['10', '1', '0'],
          correctAnswer: '0',
          selected: null,
        },
        {
          text: '98 rounded off to the nearest 10 is..',
          options: ['80', '100', '89'],
          correctAnswer: '100',
          selected: null,
        },
        {
          text: '199 rounded off to the nearest 10 is..',
          options: ['190', '100', '200'],
          correctAnswer: '200',
          selected: null,
        },
        {
          text: '94 rounded off to the nearest 10 is..',
          options: ['100', '94', '90'],
          correctAnswer: '90',
          selected: null,
        },
        {
          text: '165 rounded off to the nearest 10 is..',
          options: ['160', '170', '150'],
          correctAnswer: '170',
          selected: null,
        },
        {
          text: '445 rounded off to the nearest 10 is..',
          options: ['450', '440', '500'],
          correctAnswer: '450',
          selected: null,
        },
        {
          text: '999 rounded off to the nearest 10 is..',
          options: ['990', '1,000', '909'],
          correctAnswer: '1,000',
          selected: null,
        },
      ],
    }
  },
  computed: {
    score() {
      return this.questions.reduce((total, question) => {
        return total + (question.selected === question.correctAnswer ? 1 : 0)
      }, 0)
    },
    resultMessage() {
      const percentage = (this.score / 12) * 100
      if (percentage >= 90) return `Excellent work, ${this.studentName}! You got ${this.score}/12!`
      if (percentage >= 70) return `Good job, ${this.studentName}! You got ${this.score}/12.`
      if (percentage >= 50)
        return `Not bad, ${this.studentName}! You got ${this.score}/12. Keep practicing!`
      return `Keep practicing, ${this.studentName}! You got ${this.score}/12. You'll do better next time!`
    },
    scoreClass() {
      const percentage = (this.score / 12) * 100
      if (percentage >= 90) return 'excellent'
      if (percentage >= 70) return 'good'
      if (percentage >= 50) return 'average'
      return 'poor'
    },
  },
  watch: {
    nameEntered(newVal) {
      if (!newVal) {
        this.resetAnswers();
      }
    }
  },
  methods: {
    handleNameInput() {
      this.nameEntered = this.studentName.trim().length > 0
      this.showNameError = false
    },
    submitAnswers() {
      if (!this.studentName.trim()) {
        this.showNameError = true
        return
      }

      this.showNameError = false
      this.showFeedback = true
      this.showResults = true

      setTimeout(() => {
        const resultsElement = document.querySelector('.results')
        if (resultsElement) {
          resultsElement.scrollIntoView({ behavior: 'smooth' })
        }
      }, 100)
    },
    resetAnswers() {
      this.questions.forEach((question) => {
        question.selected = null
      })
      this.showFeedback = false
      this.showResults = false
    },
    clearFeedback() {
      if (this.showFeedback) {
        this.showFeedback = false
        this.showResults = false
      }
    },
  },
}
</script>

<style scoped>
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  20%, 60% { transform: translateX(-5px); }
  40%, 80% { transform: translateX(5px); }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { 
    opacity: 0;
    transform: translateY(20px);
  }
  to { 
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-shake {
  animation: shake 0.5s;
}

.animate-fade-in {
  animation: fadeIn 0.5s;
}

.animate-slide-up {
  animation: slideUp 0.5s;
}

.question-item.correct {
  background-color: rgba(46, 204, 113, 0.1);
  border-left: 4px solid #2ecc71;
}

.question-item.incorrect {
  background-color: rgba(231, 76, 60, 0.1);
  border-left: 4px solid #e74c3c;
}


</style>
