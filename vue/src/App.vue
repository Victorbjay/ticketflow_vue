<template>
  <div id="app">
    <component
      :is="currentComponent"
      :isLogin="currentPage === 'login'"
      @navigate="handleNavigate"
      @auth="handleAuth"
      @logout="handleLogout"
    />
  </div>
</template>

<script>
import LandingPage from './components/LandingPage.vue'
import AuthPage from './components/AuthPage.vue'
import Dashboard from './components/Dashboard.vue'
import TicketManagement from './components/TicketManagement.vue'

export default {
  name: 'App',
  components: {
    LandingPage,
    AuthPage,
    Dashboard,
    TicketManagement
  },
  data() {
    return {
      currentPage: 'home'
    }
  },
  computed: {
    currentComponent() {
      switch (this.currentPage) {
        case 'home':
          return 'LandingPage'
        case 'login':
        case 'signup':
          return 'AuthPage'
        case 'dashboard':
          return 'Dashboard'
        case 'tickets':
          return 'TicketManagement'
        default:
          return 'LandingPage'
      }
    }
  },
  mounted() {
    // Check if user is authenticated
    const session = localStorage.getItem('ticketapp_session')
    if (session && this.currentPage === 'home') {
      this.currentPage = 'dashboard'
    }
  },
  methods: {
    handleNavigate(page) {
      this.currentPage = page
    },
    handleAuth() {
      this.currentPage = 'dashboard'
    },
    handleLogout() {
      localStorage.removeItem('ticketapp_session')
      this.currentPage = 'home'
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  min-height: 100vh;
}
</style>