<template>
    <div>
      <h2 class="mt-8 text-xl font-bold">Berita yang sudah dibaca</h2>
      <div class="grid grid-cols-2 gap-4">
        <div v-for="article in readArticles" :key="article.url" class="border p-4">
          <img :src="article.urlToImage" alt="" class="w-full h-32 object-cover mb-2" />
          <h3 class="font-bold">{{ article.title }}</h3>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        readArticles: JSON.parse(localStorage.getItem('readArticles')) || [],
      };
    },
    mounted() {
      window.addEventListener('storage', () => {
        this.readArticles = JSON.parse(localStorage.getItem('readArticles')) || [];
      });
    }
  };
  </script>
  