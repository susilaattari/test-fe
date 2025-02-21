<template>
  <div class="container">
    <input v-model="searchQuery" type="text" placeholder="Cari berita..." class="search-input" />

    <div v-if="loading" class="news-grid">
    <div v-for="(layout, index) in gridLayout" :key="index" class="skeleton" :class="['news-card', layout.class]">
      <div class="news-image"></div>
      <h3 class="news-title"></h3>
      <p class="news-date"></p>
    </div>
  </div>


    <div v-else class="news-grid">
      <div v-for="(article, index) in filteredArticles" :key="article.url" @click="openArticle(article)"
        :class="['news-card', getGridClass(index)]">
        <img :src="article.urlToImage" alt="" class="news-image" />
        <div>
          <h3 class="news-name">{{ article.source.name }}</h3>
        <h4 class="news-author">Author: {{ article.author || 'Anonim'  }}</h4>
        </div>
        <h3 class="news-title">{{ article.title }}</h3>
        <p class="article-description">{{ article.description }}</p>
        <p class="news-date">{{ formatDate(article.publishedAt) }}</p>
      </div>
    </div>
    <div>
      <div class="line"></div>
      <h2 class="section-title">Berita yang sudah dibaca</h2>
      <div class="news-grid">
        <div v-for="article in readArticles" :key="article.url" class="news-card">
          <img :src="article.urlToImage" alt="" class="news-image" />
          <h3 class="news-title">{{ article.title }}</h3>

        </div>
      </div>
    </div>
    

  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      articles: [],
      readArticles: JSON.parse(localStorage.getItem('readArticles')) || [],
      searchQuery: '',
      loading: true,
      gridLayout: [
        { class: 'large' },
        { class: 'small' }, { class: 'small' },
        { class: 'medium' }, { class: 'medium' },
        { class: 'small' }, { class: 'small' }, { class: 'small' },
        { class: 'large' }
      ]
    };
  },
  computed: {
    filteredArticles() {
      return this.articles.filter(article =>
        article.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    }
  },
  methods: {
    async fetchNews() {
      try {
        const apiKey = import.meta.env.NEWS_API_KEY;
        const response = await axios.get('https://newsapi.org/v2/top-headlines?country=us&apiKey=6e148079e62b47c0996914614d98888f');
        this.articles = response.data.articles;
      } catch (error) {
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    openArticle(article) {
      window.open(article.url, '_blank');
      if (!this.readArticles.find(a => a.url === article.url)) {
        this.readArticles.push(article);
        localStorage.setItem('readArticles', JSON.stringify(this.readArticles));
      }
    },
    formatDate(dateStr) {
      const options = { weekday: 'short', day: 'numeric', month: 'long', hour: '2-digit', minute: '2-digit' };
      return new Date(dateStr).toLocaleDateString('id-ID', options);
    },
    getGridClass(index) {
      const layout = ['big', 'small', 'small', 'medium', 'medium', 'small', 'small', 'large'];
      return layout[index % layout.length];
    }
  },
  mounted() {
    this.fetchNews();
  }
};
</script>

<style>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px;
}

.search-input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 20px;
}

.line {
  width: 100%;
  border-top: 2px dashed #158edf;
  margin: 16px 0;
}

.skeleton {
  height: 80px;
  width: 100%;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
  border-radius: 8px;
}

@keyframes loading {
  0% {
    background-position: 200% 0;
  }
  100% {
    background-position: -200% 0;
  }
}

/* Grid Layout */
.news-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
}

/* Responsive Grid */
@media (max-width: 1024px) {
  .news-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .news-grid {
    grid-template-columns: 1fr;
  }
}

/* Card */
.news-card {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 8px;
  transition: box-shadow 0.3s;
  cursor: pointer;
  display: flex;
  flex-direction: column;
}

.news-card:hover {
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
}

.news-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 5px;
}

/* Typography */
.news-name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 2px;
}

.news-author {
  font-size: 14px;
  color: gray;
  margin-bottom: 2px;
}

.news-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 2px;
}

.news-date {
  font-size: 14px;
  color: #666;
  margin-bottom: 2px;
}

/* Grid Item Sizes */
.big {
  grid-column: span 2;
  grid-row: span 2;
}

.medium {
  grid-column: span 1;
  grid-row: span 1;
}

.small {
  grid-column: span 1;
  grid-row: span 1;
}

.large {
  grid-column: span 2;
  grid-row: span 2;
}
.article-description {
  text-align: justify;
  font-family: "Georgia", serif;
  font-size: 14px;
  line-height: 1.6;
}

</style>
