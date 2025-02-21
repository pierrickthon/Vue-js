<template>
    <div class="article-card">
      <img :src="article.image" :alt="article.titre" />
      <h3>{{ article.titre }}</h3>
      <p class="description">{{ article.description }}</p>
      <p class="prix">{{ article.prix.toFixed(2) }} €</p>
      <div class="actions">
        <router-link :to="`/article/${article.id}`" class="btn btn-info">Détails</router-link>
        <button @click="ajouterAuPanier" class="btn btn-success">Ajouter au panier</button>
        <button v-if="showDelete" @click="supprimerArticle" class="btn btn-danger">Supprimer</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ArticleItem',
    props: {
      article: {
        type: Object,
        required: true
      },
      showDelete: {
        type: Boolean,
        default: false
      }
    },
    methods: {
      ajouterAuPanier() {
        this.$store.commit('ajouterAuPanier', this.article)
      },
      supprimerArticle() {
        if (confirm(`Êtes-vous sûr de vouloir supprimer "${this.article.titre}" ?`)) {
          this.$store.commit('supprimerArticle', this.article.id)
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .article-card {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 15px;
    margin-bottom: 20px;
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s;
  }
  
  .article-card:hover {
    transform: translateY(-5px);
  }
  
  .article-card img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 4px;
    margin-bottom: 10px;
  }
  
  .description {
    color: #666;
    margin-bottom: 10px;
  }
  
  .prix {
    font-weight: bold;
    font-size: 18px;
    color: #42b983;
    margin-bottom: 15px;
  }
  
  .actions {
    display: flex;
    gap: 10px;
  }
  
  .btn {
    padding: 8px 12px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
    text-decoration: none;
    text-align: center;
  }
  
  .btn-info {
    background-color: #2196F3;
    color: white;
  }
  
  .btn-success {
    background-color: #42b983;
    color: white;
  }
  
  .btn-danger {
    background-color: #f44336;
    color: white;
  }
  </style>