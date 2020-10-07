<template>
  <div>
    <AppSearchInput />
    <h1>Блог</h1>
    <label>Sort by</label>
    <select v-model="sortBy">
      <option
        v-for="option in sortOptions"
        v-bind:value="option.value"
        :key="option.value"
      >
        {{ option.text }}
      </option>
    </select>
    <ul class="container">
      <li v-for="article of articles" :key="article.slug">
        <NuxtLink :to="article.slug">
          <img :src="article.img" class="article-image" />
          <div>
            <h2>{{ article.title }}</h2>
            <p>автор: {{ article.author.name }}</p>
            <p>{{ article.description }}</p>
          </div>
        </NuxtLink>
        <hr />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sortBy: "createdAt",
      sortOptions: [
        { text: "Date", value: "createdAt" },
        { text: "Title", value: "title" }
      ]
    };
  },
  async asyncData({ $content, params }) {
    let articles = await $content("articles", params.slug)
      .only(["title", "description", "img", "slug", "author"])
      .sortBy("createdAt", "asc")
      .fetch();
    // const articles = await fetchArticles($content, sortBy, params.slug);

    return {
      articles
    };
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.article-image {
  max-width: 600px;
  height: auto;
}
</style>
