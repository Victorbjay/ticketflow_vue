<template>
  <div class="dashboard">
    <!-- Navigation -->
    <nav class="nav">
      <div class="nav-container">
        <div class="nav-brand">TicketFlow</div>
        <div class="nav-actions">
          <span class="user-email">{{ user?.email }}</span>
          <button class="btn-logout" @click="$emit('logout')">
            Logout
          </button>
        </div>
      </div>
    </nav>

    <!-- Main Content -->
    <div class="container">
      <!-- Welcome Section -->
      <div class="welcome">
        <div class="circle"></div>
        <h1>Welcome back! 👋</h1>
        <p>Here's what's happening with your tickets today</p>
      </div>

      <!-- Stats Grid -->
      <div class="stats-grid">
        <div
          v-for="stat in stats"
          :key="stat.label"
          class="stat-card"
          :style="{ borderColor: stat.color + '33' }"
        >
          <div class="stat-circle" :style="{ background: `radial-gradient(circle, ${stat.color}33, transparent)` }"></div>
          <div class="stat-icon">{{ stat.icon }}</div>
          <div class="stat-value" :style="{ color: stat.color }">{{ stat.value }}</div>
          <div class="stat-label">{{ stat.label }}</div>
        </div>
      </div>

      <!-- Quick Actions -->
      <div class="quick-actions">
        <h2>Quick Actions</h2>
        <button class="btn-action" @click="$emit('navigate', 'tickets')">
          <span>🎫</span> Manage Tickets
        </button>
      </div>
    </div>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <p>© 2025 TicketFlow • Built for Frontend Wizards Stage 2</p>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  data() {
    return {
      user: null,
      tickets: []
    }
  },
  computed: {
    stats() {
      const totalTickets = this.tickets.length
      const openTickets = this.tickets.filter(t => t.status === 'open').length
      const inProgressTickets = this.tickets.filter(t => t.status === 'in_progress').length
      const resolvedTickets = this.tickets.filter(t => t.status === 'closed').length

      return [
        { icon: '📊', value: totalTickets, label: 'Total Tickets', color: '#6366f1' },
        { icon: '🟢', value: openTickets, label: 'Open Tickets', color: '#10b981' },
        { icon: '🟡', value: inProgressTickets, label: 'In Progress', color: '#f59e0b' },
        { icon: '✓', value: resolvedTickets, label: 'Resolved', color: '#94a3b8' }
      ]
    }
  },
  mounted() {
    const session = localStorage.getItem('ticketapp_session')
    if (!session) {
      this.$emit('navigate', 'login')
      return
    }
    const sessionData = JSON.parse(session)
    this.user = sessionData.user

    const storedTickets = localStorage.getItem('ticketapp_tickets')
    if (storedTickets) {
      this.tickets = JSON.parse(storedTickets)
    }
  }
}
</script>

<style scoped>
.dashboard {
  min-height: 100vh;
  background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
  color: #f1f5f9;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

/* Navigation */
.nav {
  background: rgba(30, 41, 59, 0.8);
  backdrop-filter: blur(10px);
  position: sticky;
  top: 0;
  z-index: 1000;
  border-bottom: 1px solid rgba(99, 102, 241, 0.1);
}

.nav-container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 1.5rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.nav-brand {
  font-size: 1.75rem;
  font-weight: 700;
  background: linear-gradient(135deg, #6366f1, #ec4899);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.nav-actions {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}

.user-email {
  color: #94a3b8;
  font-size: 0.875rem;
}

.btn-logout {
  padding: 0.625rem 1.5rem;
  background: rgba(239, 68, 68, 0.1);
  color: #ef4444;
  border: 1px solid rgba(239, 68, 68, 0.3);
  border-radius: 8px;
  font-size: 0.875rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-logout:hover {
  background: rgba(239, 68, 68, 0.2);
}

/* Main Content */
.container {
  max-width: 1440px;
  margin: 0 auto;
  padding: 3rem 2rem;
}

.welcome {
  margin-bottom: 3rem;
  position: relative;
  overflow: hidden;
}

.welcome .circle {
  position: absolute;
  top: -50px;
  right: -50px;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(99, 102, 241, 0.15), transparent);
  filter: blur(40px);
  z-index: 0;
}

.welcome h1 {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 700;
  margin-bottom: 0.5rem;
  position: relative;
  z-index: 1;
}

.welcome p {
  color: #94a3b8;
  font-size: 1.125rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin-bottom: 3rem;
}

.stat-card {
  background: rgba(30, 41, 59, 0.6);
  border: 1px solid;
  border-radius: 20px;
  padding: 2rem;
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
  cursor: pointer;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
}

.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 32px rgba(99, 102, 241, 0.2);
}

.stat-circle {
  position: absolute;
  top: -20px;
  right: -20px;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  filter: blur(20px);
}

.stat-icon {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

.stat-value {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 0.25rem;
}

.stat-label {
  color: #94a3b8;
  font-size: 0.875rem;
}

.quick-actions {
  background: rgba(30, 41, 59, 0.6);
  border: 1px solid rgba(99, 102, 241, 0.2);
  border-radius: 20px;
  padding: 2.5rem;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
}

.quick-actions h2 {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 1.5rem;
}

.btn-action {
  padding: 1rem 2rem;
  background: linear-gradient(135deg, #6366f1, #8b5cf6);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(99, 102, 241, 0.4);
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.btn-action:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(99, 102, 241, 0.5);
}

/* Footer */
.footer {
  background: rgba(15, 23, 42, 0.8);
  border-top: 1px solid rgba(99, 102, 241, 0.1);
  padding: 2rem;
  margin-top: 4rem;
  text-align: center;
  color: #64748b;
  font-size: 0.875rem;
}
</style>