<template>
    <div class="liste-articles">
      <div v-if="articlesFiltres.length === 0" class="no-results">
        Aucun article ne correspond à votre recherche.
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
        v-if="articlesFiltres.length > articlesParPage"
        :total="articlesFiltres.length"
        :perPage="articlesParPage"
        :currentPage="pageCourante"
        @pageChanged="changePage"
      />
    </div>
  </template>
  
  <script>
  import ArticleItem from './ArticleItem.vue'  
  export default {
    name: 'ListeArticles',
    components: {
      ArticleItem
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
        if (!this.recherche) return this.articles
        const recherche = this.recherche.toLowerCase()
        return this.articles.filter(article => 
          article.titre.toLowerCase().includes(recherche) || 
          article.description.toLowerCase().includes(recherche)
        )
      },
      articlesPage() {
        const startIndex = (this.pageCourante - 1) * this.articlesParPage
        return this.articlesFiltres.slice(startIndex, startIndex + this.articlesParPage)
      }
    },
    methods: {
      changePage(page) {
        this.pageCourante = page
      }
    },
    watch: {
      recherche() {
        // Réinitialiser la pagination quand la recherche change
        this.pageCourante = 1
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
  </style>