<template>
    <div class="worksheet-container">
      <h1>Rounding Off to Nearest 10</h1>
      
      <div class="header-section">
        <div class="name-input">
          <label for="studentName">Name:</label>
          <input 
            type="text" 
            id="studentName" 
            v-model="studentName" 
            placeholder="Enter your name"
            :class="{ 'input-error': showNameError }"
            @input="handleNameInput"
          >
        </div>
        <div class="score-display" v-if="showResults">
          Score: {{ score }}/12
        </div>
      </div>
  
      <div class="instructions">Circle the correct answers.</div>
  
      <div class="questions-container">
        <div 
          v-for="(question, index) in questions" 
          :key="index" 
          class="question-item"
          :class="{ 
            'correct': showFeedback && question.selected === question.correctAnswer, 
            'incorrect': showFeedback && question.selected && question.selected !== question.correctAnswer 
          }"
        >
          <div class="question-text">{{ question.text }}</div>
          <div class="options-container">
            <label 
              v-for="(option, optIndex) in question.options" 
              :key="optIndex"
              :class="{ 
                'selected': question.selected === option,
                'disabled': !nameEntered
              }"
            >
              <input 
                type="radio" 
                :name="'q' + index" 
                :value="option" 
                v-model="question.selected"
                @change="clearFeedback(index)"
                :disabled="!nameEntered"
              >
              {{ option }}
            </label>
          </div>
          <div class="feedback" v-if="showFeedback && question.selected">
            {{ question.selected === question.correctAnswer ? '✓ Correct!' : '✗ Incorrect' }}
          </div>
        </div>
      </div>
  
      <div class="actions">
        <button 
          @click="resetAnswers" 
          class="reset-btn"
          :disabled="!nameEntered"
        >
          Reset Answers
        </button>
        <button 
          @click="submitAnswers" 
          class="submit-btn"
          :disabled="!nameEntered"
        >
          Submit
        </button>
      </div>
  
      <div class="results" v-if="showResults">
        <div class="result-message" :class="scoreClass">
          {{ resultMessage }}
        </div>
      </div>
  
      <div class="copyright">copyright: www.mathinenglish.com</div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Worksheet',
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
            selected: null
          },
          {
            text: '45 rounded off to the nearest 10 is..',
            options: ['50', '45', '40'],
            correctAnswer: '50',
            selected: null
          },
          {
            text: '75 rounded off to the nearest 10 is..',
            options: ['70', '80', '175'],
            correctAnswer: '80',
            selected: null
          },
          {
            text: '19 rounded to the nearest 10 is..',
            options: ['20', '10', '19'],
            correctAnswer: '20',
            selected: null
          },
          {
            text: '64 rounded off to the nearest 10 is..',
            options: ['64', '70', '60'],
            correctAnswer: '60',
            selected: null
          },
          {
            text: '0 rounded off to the nearest 10 is..',
            options: ['10', '1', '0'],
            correctAnswer: '0',
            selected: null
          },
          {
            text: '98 rounded off to the nearest 10 is..',
            options: ['80', '100', '89'],
            correctAnswer: '100',
            selected: null
          },
          {
            text: '199 rounded off to the nearest 10 is..',
            options: ['190', '100', '200'],
            correctAnswer: '200',
            selected: null
          },
          {
            text: '94 rounded off to the nearest 10 is..',
            options: ['100', '94', '90'],
            correctAnswer: '90',
            selected: null
          },
          {
            text: '165 rounded off to the nearest 10 is..',
            options: ['160', '170', '150'],
            correctAnswer: '170',
            selected: null
          },
          {
            text: '445 rounded off to the nearest 10 is..',
            options: ['450', '440', '500'],
            correctAnswer: '450',
            selected: null
          },
          {
            text: '999 rounded off to the nearest 10 is..',
            options: ['990', '1,000', '909'],
            correctAnswer: '1,000',
            selected: null
          }
        ]
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
        if (percentage >= 50) return `Not bad, ${this.studentName}! You got ${this.score}/12. Keep practicing!`
        return `Keep practicing, ${this.studentName}! You got ${this.score}/12. You'll do better next time!`
      },
      scoreClass() {
        const percentage = (this.score / 12) * 100
        if (percentage >= 90) return 'excellent'
        if (percentage >= 70) return 'good'
        if (percentage >= 50) return 'average'
        return 'poor'
      }
    },
    methods: {
        handleNameInput() {
      this.nameEntered = this.studentName.trim().length > 0;
      this.showNameError = false;
    },
    submitAnswers() {
      if (!this.studentName.trim()) {
        this.showNameError = true;
        return;
      }
      
      this.showNameError = false;
      this.showFeedback = true;
      this.showResults = true;
      
      setTimeout(() => {
        const resultsElement = document.querySelector('.results');
        if (resultsElement) {
          resultsElement.scrollIntoView({ behavior: 'smooth' });
        }
      }, 100);
    },
    resetAnswers() {
      this.questions.forEach(question => {
        question.selected = null;
      });
      this.showFeedback = false;
      this.showResults = false;
    },
    clearFeedback() {
      if (this.showFeedback) {
        this.showFeedback = false;
        this.showResults = false;
      }
    }
    }
  }
  </script>
  
  <style scoped>
  .worksheet-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    font-family: 'Arial', sans-serif;
    background-color: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    color: #2c3e50;
    text-align: center;
    margin-bottom: 20px;
  }
  
  .header-section {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding: 15px;
    background-color: #e8f4fc;
    border-radius: 8px;
  }
  
  .name-input input {
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 16px;
    width: 200px;
  }
  
  .input-error {
    border-color: #e74c3c !important;
    animation: shake 0.5s;
  }
  
  @keyframes shake {
    0%, 100% { transform: translateX(0); }
    20%, 60% { transform: translateX(-5px); }
    40%, 80% { transform: translateX(5px); }
  }
  
  .score-display {
    font-weight: bold;
    font-size: 18px;
    color: #2c3e50;
  }
  
  .instructions {
    font-style: italic;
    margin-bottom: 20px;
    text-align: center;
    color: #7f8c8d;
  }
  
  .questions-container {
    display: grid;
    grid-template-columns: 1fr;
    gap: 15px;
    margin-bottom: 30px;
  }
  
  .question-item {
    padding: 15px;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
  }
  
  .question-item:hover {
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  }
  
  .question-item.correct {
    background-color: rgba(46, 204, 113, 0.1);
    border-left: 4px solid #2ecc71;
  }
  
  .question-item.incorrect {
    background-color: rgba(231, 76, 60, 0.1);
    border-left: 4px solid #e74c3c;
  }
  
  .question-text {
    font-weight: bold;
    margin-bottom: 10px;
  }
  
  .options-container {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }
  
  .options-container label {
    display: flex;
    align-items: center;
    padding: 8px 15px;
    border: 1px solid #ddd;
    border-radius: 20px;
    cursor: pointer;
    transition: all 0.2s;
  }
  
  .options-container label:hover {
    background-color: #f0f0f0;
  }
  
  .options-container label.selected {
    background-color: #3498db;
    color: white;
    border-color: #3498db;
  }
  
  .options-container input[type="radio"] {
    margin-right: 8px;
    cursor: pointer;
  }
  
  .feedback {
    margin-top: 10px;
    font-weight: bold;
    animation: fadeIn 0.5s;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  
  .actions {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-bottom: 30px;
  }
  
  button {
    padding: 10px 25px;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .reset-btn {
    background-color: #95a5a6;
    color: white;
  }
  
  .reset-btn:hover {
    background-color: #7f8c8d;
  }
  
  .submit-btn {
    background-color: #3498db;
    color: white;
  }
  
  .submit-btn:hover {
    background-color: #2980b9;
  }
  
  .results {
    text-align: center;
    margin-bottom: 30px;
    padding: 20px;
    border-radius: 8px;
    animation: slideUp 0.5s;
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
  
  .result-message {
    font-size: 18px;
    font-weight: bold;
    padding: 15px;
    border-radius: 5px;
  }
  
  .excellent {
    background-color: rgba(46, 204, 113, 0.2);
    color: #27ae60;
  }
  
  .good {
    background-color: rgba(52, 152, 219, 0.2);
    color: #2980b9;
  }
  
  .average {
    background-color: rgba(241, 196, 15, 0.2);
    color: #f39c12;
  }
  
  .poor {
    background-color: rgba(231, 76, 60, 0.2);
    color: #c0392b;
  }
  
  .copyright {
    text-align: center;
    color: #95a5a6;
    font-size: 14px;
    margin-top: 20px;
  }
  
  @media (max-width: 600px) {
    .header-section {
      flex-direction: column;
      gap: 15px;
    }
    
    .options-container {
      flex-direction: column;
      gap: 8px;
    }
    
    .actions {
      flex-direction: column;
      align-items: center;
    }
    
    button {
      width: 100%;
      max-width: 200px;
    }
  }
  </style>