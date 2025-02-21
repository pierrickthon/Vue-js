<template>
    <div class="formulaire-article">
      <h2>{{ titre }}</h2>
      <form @submit.prevent="soumettreFormulaire">
        <div class="form-group">
          <label for="titre">Titre</label>
          <input 
            type="text" 
            id="titre" 
            v-model="formData.titre" 
            required
          />
        </div>
        
        <div class="form-group">
          <label for="description">Description</label>
          <textarea 
            id="description" 
            v-model="formData.description" 
            rows="4" 
            required
          ></textarea>
        </div>
        
        <div class="form-group">
          <label for="prix">Prix (€)</label>
          <input 
            type="number" 
            id="prix" 
            v-model.number="formData.prix" 
            min="0.01" 
            step="0.01" 
            required
          />
        </div>
        
        <div class="form-group">
          <label for="image">URL de l'image</label>
          <input 
            type="url" 
            id="image" 
            v-model="formData.image" 
            required
          />
        </div>
        
        <div v-if="formData.image" class="image-preview">
          <h3>Aperçu de l'image:</h3>
          <img :src="formData.image" alt="Aperçu de l'article" />
        </div>
        
        <div class="form-actions">
          <button type="submit" class="btn btn-primary">{{ boutonTexte }}</button>
          <button type="button" @click="reinitialiserFormulaire" class="btn btn-secondary">Réinitialiser</button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'FormulaireArticle',
    props: {
      titre: {
        type: String,
        default: 'Ajouter un nouvel article'
      },
      boutonTexte: {
        type: String,
        default: 'Ajouter'
      }
    },
    data() {
      return {
        formData: {
          titre: '',
          description: '',
          prix: '',
          image: 'https://via.placeholder.com/200'
        }
      }
    },
    methods: {
      soumettreFormulaire() {
        // Vérification basique
        if (!this.formData.titre || !this.formData.description || !this.formData.prix || !this.formData.image) {
          alert('Veuillez remplir tous les champs')
          return
        }
        
        // Cloner l'objet pour éviter les mutations directes
        const nouvelArticle = { ...this.formData }
        
        // Émettre l'événement avec les données du formulaire
        this.$emit('soumettre', nouvelArticle)
        
        // Réinitialiser le formulaire
        this.reinitialiserFormulaire()
      },
      reinitialiserFormulaire() {
        this.formData = {
          titre: '',
          description: '',
          prix: '',
          image: 'https://via.placeholder.com/200'
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .formulaire-article {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .form-group {
    margin-bottom: 20px;
  }
  
  label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
  }
  
  input, textarea {
    width: 100%;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 16px;
  }
  
  textarea {
    resize: vertical;
  }
  
  .image-preview {
    margin-top: 20px;
    text-align: center;
  }
  
  .image-preview img {
    max-width: 100%;
    max-height: 200px;
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 5px;
  }
  
  .form-actions {
    display: flex;
    justify-content: space-between;
    margin-top: 30px;
  }
  
  .btn {
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
  }
  
  .btn-primary {
    background-color: #42b983;
    color: white;
  }
  
  .btn-secondary {
    background-color: #6c757d;
    color: white;
  }
  </style>