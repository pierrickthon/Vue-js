<template>
    <div class="barre-recherche">
      <input 
        type="text" 
        v-model="termeRecherche" 
        placeholder="Rechercher un article..." 
        @input="rechercherArticles"
      />
    </div>
  </template>
  
  <script>
  export default {
    name: 'BarreRecherche',
    data() {
      return {
        termeRecherche: ''
      }
    },
    methods: {
      rechercherArticles() {
        this.$emit('recherche', this.termeRecherche)
      }
    }
  }
  </script>
  
  <style scoped>
  .barre-recherche {
    margin-bottom: 20px;
  }
  
  input {
    width: 100%;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 16px;
  }
  </style>