<template>
  <div class="main">
    <div class="navbar">
      <h1 class="navbar-title">News Listing App</h1>
    </div>
    <NewsList />
    <ReadArticles />
  </div>
</template>

<script>
import NewsList from './components/NewsList.vue';
// import ReadArticles from './components/ReadArticles.vue';

export default {
  components: {
    NewsList,
    // ReadArticles
  }
};
</script>

<style>

.navbar {
  background-color: rgb(15, 98, 150);
  padding: 15px 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  /* box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1); */
  /* position: sticky; */
  top: 0;
  z-index: 1000;
  margin: 0;
}

.navbar-title {
  color: white;
  font-size: 1.5rem;
  font-weight: bold;
  text-transform: uppercase;
}
</style>
