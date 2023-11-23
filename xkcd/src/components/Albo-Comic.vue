<template>
  <div class="albo-container">
    <!-- Comic Details -->
    <h1>{{ comic.safe_title }}</h1>
    <p>Date: {{ comic.day }}-{{ comic.month }}-{{ comic.year }}</p>
    <p>Number: {{ comic.num }}</p>
  
    <!-- Comic Image -->
    <img :src="comic.img" :alt="comic.alt" />

    <!-- Navigation Buttons -->
    <div class="navigation-buttons">
      <button @click="prevComic" :disabled="comic.num <= 1">PREVIUOS</button>
      <button @click="nextComic" >NEXT</button>
    </div>
  </div>
</template>
<style>
.albo-container {
  text-align: center;
  padding: 20px;
}

.comic-details {
  margin-bottom: 20px;
}

.comic-image img {
  max-width: 100%;
  height: auto;
  margin: 20px 0;
}

.navigation-buttons button {
  padding: 10px 15px;
  margin: 5px;
  cursor: pointer;
  border: none;
  background-color: #007BFF;
  color: #FFF;
  border-radius: 5px;
  transition: background-color 0.3s;
}

.navigation-buttons button:disabled {
  background-color: #AAA;
  cursor: not-allowed;
}
</style>

<script>

import axios from 'axios';

export default {
name: 'Albo-Comic',
data() {
  return {
    comic: {},
    maxComicNumber: 0 // Maximum comic number to ensure we don't request beyond the available comics
  }
},
methods: {
  // Fetch details of a specific comic
  fetchComic(comicNumber = 100) { 
    axios.get(`https://xkcd.com/${comicNumber}/info.0.json`)
      .then(response => {
        this.comic = response.data;
        this.maxComicNumber = this.comic.num;  // Update the maxComicNumber
      })
      .catch(error => {
        console.error("Error fetching the comic:", error);
      });
  },
  
  // Go to the next comic
  nextComic() {
    
      this.fetchComic(this.comic.num + 1);
    
  },
  
  // Go to the previous comic
  prevComic() {
    if (this.comic.num > 1) {
      this.fetchComic(this.comic.num - 1);
    }
  }
},
// Fetch the comic when the component is mounted
mounted() {
  this.fetchComic();
}
}
</script>
