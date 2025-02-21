<template>
    <div class="liste-articles">
      <div v-if="!articlesFiltres?.length" class="no-results">
        <h1>Aucun article trouvé</h1>
      </div>
      <div v-else class="articles-grid">
        <ArticleItem 
          v-for="article in articlesPage" 
          :key="article.id" 
          :article="article"
          :showDelete="showDelete"
        />
      </div>
      <Pagination 
        v-if="articlesFiltres && articlesFiltres.length > articlesParPage"
        :total="articlesFiltres.length"
        :perPage="articlesParPage"
        :currentPage="pageCourante"
        @pageChanged="changePage"
      />
    </div>
  </template>
  
  <script>
  import ArticleItem from './ArticleItem.vue'
  import Pagination from './Pagination.vue'
  
  export default {
    name: 'ListeArticles',
    components: {
      ArticleItem,
      Pagination
    },
    props: {
      articles: {
        type: Array,
        required: true
      },
      recherche: {
        type: String,
        default: ''
      },
      showDelete: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        pageCourante: 1,
        articlesParPage: 3
      }
    },
    computed: {
      articlesFiltres() {
        if (!this.recherche?.trim()) return this.articles;
        const searchTerm = this.recherche.toLowerCase().trim();
        return this.articles.filter(article => 
          article.titre?.toLowerCase().includes(searchTerm)
        );
      },
      articlesPage() {
        const start = (this.pageCourante - 1) * this.articlesParPage;
        const end = start + this.articlesParPage;
        return this.articlesFiltres?.slice(start, end);
      }
    },
    methods: {
      changePage(page) {
        this.pageCourante = page;
      }
    }
  }
  </script>
  
  <style scoped>
  .articles-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;
  }
  
  .no-results {
    text-align: center;
    padding: 30px;
    background-color: #f8f8f8;
    border-radius: 8px;
    margin: 20px 0;
  }

  .delete-btn {
  background-color: red;
  color: white;
  border: none;
  padding: 5px;
  cursor: pointer;
  }
  </style>