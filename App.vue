<template>
  <div class="app-container">
    <header>
      <nav>
        <router-link to="/login">Login</router-link> |
        <template v-if="isLoggedIn">
          <router-link to="/">Accueil</router-link> |
          <router-link to="/articles">Articles</router-link> |
          <router-link to="/ajouter">Ajouter un article</router-link> |
          <router-link to="/panier">Panier ({{ nombreArticlesPanier }})</router-link> |
          <router-link to="/admin">Admin</router-link> |
          <a href="#" @click.prevent="deconnexion" class="logout-link">Déconnexion</a>
        </template>
      </nav>
    </header>
    <main>
      <router-view />
    </main>
    <footer>
      <p>Mini-application Vue.js - 2025</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  computed: {
    nombreArticlesPanier() {
      return this.$store.state.panier.length;
    },
    isLoggedIn() {
      return this.$store.state.isLoggedIn;
    }
  },
  methods: {
    deconnexion() {
      this.$store.dispatch('logout');
      this.$router.push('/');
    }
  }
}
</script>

<style>
.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

header {
  margin-bottom: 30px;
}

nav {
  padding: 15px 0;
  border-bottom: 1px solid #ddd;
}

nav a {
  margin: 0 10px;
  text-decoration: none;
  color: #333;
}

nav a.router-link-active {
  font-weight: bold;
  color: #42b983;
}

.logout-link {
  color: #dc3545;
  cursor: pointer;
}

.logout-link:hover {
  text-decoration: underline;
}

footer {
  margin-top: 50px;
  padding: 20px 0;
  text-align: center;
  border-top: 1px solid #ddd;
}
</style>