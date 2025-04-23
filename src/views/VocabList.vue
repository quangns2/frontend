<template>
    <div class="container py-5">
      <h1 class="text-center text-primary mb-4">📘 Vocab Builder</h1>
      <!-- Form thêm từ -->
      <form @submit.prevent="addWord" class="row g-2 mb-4">
        <div class="col-md-4">
            <input v-model="newWord.word" type="text" class="form-control" placeholder="English" required>
        </div>
        <div class="col-md-4">
            <input v-model="newWord.meaning" type="text" class="form-control" placeholder="German" required>
        </div>
        <div class="col-md-4">
          <input v-model="newWord.french" type="text" class="form-control" placeholder="France">
        </div>
        <div class="col-md-2 d-grid">
        
          <button class="btn btn-primary">Add</button>
        </div>
      </form>
  
      <!-- Danh sách từ -->
      <ul class="list-group">
        <li v-for="(word, index) in words" :key="word._id" class="list-group-item d-flex justify-content-between align-items-center">
          <div>
            <strong>{{ word.word }}</strong> – <span class="text-muted">{{ word.meaning }}</span> - <span class="text-muted">{{ word.french }}</span>
          </div>
          <div>
            <button @click="editWord(index)" class="btn btn-sm btn-outline-primary me-2">Edit</button>
            <button @click="deleteWord(word._id)" class="btn btn-sm btn-outline-danger">Delete</button>
          </div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        newWord: { word: '', meaning: '', french: ''},
        words: [],
      };
    },
    methods: {
      async fetchWords() {
        const res = await axios.get('https://express-nodejs-api.onrender.com/api/vocab');
        // const res = await axios.get('http://localhost:3000/api/vocab');
        this.words = res.data;
      },
      async addWord() {
        console.log(this.newWord); 
        const res = await axios.post('https://express-nodejs-api.onrender.com/api/vocab', this.newWord);
        this.words.push(res.data);
        this.newWord.word = ''; 
        this.newWord.meaning = '';
        this.newWord.french = '';
      },
      async deleteWord(id) {
        await axios.delete(`https://express-nodejs-api.onrender.com/api/vocab/${id}`);
        this.words = this.words.filter(word => word._id !== id);
      },
      editWord(index) {
        const word = this.words[index];
        this.newWord = { ...word };
        this.deleteWord(word._id);
      }
    },
    mounted() {
      this.fetchWords();
    }
  };
  </script>
