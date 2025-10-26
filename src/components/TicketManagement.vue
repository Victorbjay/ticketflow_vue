<template>
  <div class="ticket-management">
    <!-- Toast -->
    <Toast v-if="toast" :message="toast.message" :type="toast.type" @close="toast = null" />

    <!-- Navigation -->
    <nav class="nav">
      <div class="nav-container">
        <div class="nav-left">
          <div class="nav-brand" @click="$emit('navigate', 'dashboard')">ResolveHub</div>
          <button class="btn-back" @click="$emit('navigate', 'dashboard')">
            ← Dashboard
          </button>
        </div>
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
      <!-- Header -->
      <div class="header">
        <h1>Ticket Management</h1>
        <button class="btn-create" @click="handleCreate">
          <span class="plus-icon">+</span>
          Create Ticket
        </button>
      </div>

      <!-- Empty State -->
      <div v-if="tickets.length === 0" class="empty-state">
        <div class="empty-icon">🎫</div>
        <h3>No tickets yet</h3>
        <p>Create your first ticket to get started</p>
        <button class="btn-create" @click="handleCreate">
          Create First Ticket
        </button>
      </div>

      <!-- Tickets Grid -->
      <div v-else class="tickets-grid">
        <div
          v-for="ticket in tickets"
          :key="ticket.id"
          class="ticket-card"
        >
          <!-- Status Badge -->
          <div
            class="status-badge"
            :style="{
              background: getStatusColor(ticket.status).bg,
              borderColor: getStatusColor(ticket.status).border,
              color: getStatusColor(ticket.status).text
            }"
          >
            {{ getStatusLabel(ticket.status) }}
          </div>

          <!-- Title -->
          <h3>{{ ticket.title }}</h3>

          <!-- Description -->
          <p v-if="ticket.description" class="description">
            {{ ticket.description.length > 120 ? ticket.description.substring(0, 120) + '...' : ticket.description }}
          </p>

          <!-- Priority -->
          <div v-if="ticket.priority" class="priority">
            <span>Priority:</span>
            <span class="priority-value">{{ ticket.priority }}</span>
          </div>

          <!-- Actions -->
          <div class="actions">
            <button class="btn-edit" @click="handleEdit(ticket)">
              Edit
            </button>
            <button class="btn-delete" @click="handleDelete(ticket)">
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Create/Edit Modal -->
    <Modal :isOpen="isModalOpen" @close="isModalOpen = false">
      <div class="modal-content">
        <h2>{{ editingTicket ? 'Edit Ticket' : 'Create New Ticket' }}</h2>

        <form @submit.prevent="handleSubmit">
          <!-- Title -->
          <div class="form-group">
            <label for="title">Title *</label>
            <input
              type="text"
              id="title"
              v-model="formData.title"
              placeholder="Enter ticket title"
              :class="{ error: errors.title }"
              @input="clearError('title')"
            />
            <div v-if="errors.title" class="error-message">
              <span>⚠</span> {{ errors.title }}
            </div>
          </div>

          <!-- Description -->
          <div class="form-group">
            <label for="description">Description</label>
            <textarea
              id="description"
              v-model="formData.description"
              placeholder="Enter ticket description (optional)"
              rows="4"
              :class="{ error: errors.description }"
              @input="clearError('description')"
            ></textarea>
            <div v-if="errors.description" class="error-message">
              <span>⚠</span> {{ errors.description }}
            </div>
          </div>

          <!-- Status -->
          <div class="form-group">
            <label for="status">Status *</label>
            <select
              id="status"
              v-model="formData.status"
              :class="{ error: errors.status }"
              @change="clearError('status')"
            >
              <option value="open">Open</option>
              <option value="in_progress">In Progress</option>
              <option value="closed">Closed</option>
            </select>
            <div v-if="errors.status" class="error-message">
              <span>⚠</span> {{ errors.status }}
            </div>
          </div>

          <!-- Priority -->
          <div class="form-group">
            <label for="priority">Priority</label>
            <select id="priority" v-model="formData.priority">
              <option value="low">Low</option>
              <option value="medium">Medium</option>
              <option value="high">High</option>
            </select>
          </div>

          <!-- Actions -->
          <div class="modal-actions">
            <button type="button" class="btn-cancel" @click="isModalOpen = false">
              Cancel
            </button>
            <button type="submit" class="btn-submit">
              {{ editingTicket ? 'Update Ticket' : 'Create Ticket' }}
            </button>
          </div>
        </form>
      </div>
    </Modal>

    <!-- Delete Confirmation Modal -->
    <Modal :isOpen="deleteConfirm !== null" @close="deleteConfirm = null">
      <div class="modal-content delete-modal">
        <div class="delete-icon">⚠️</div>
        <h2>Delete Ticket?</h2>
        <p>Are you sure you want to delete "{{ deleteConfirm?.title }}"? This action cannot be undone.</p>
        <div class="modal-actions">
          <button class="btn-cancel" @click="deleteConfirm = null">
            Cancel
          </button>
          <button class="btn-delete-confirm" @click="confirmDelete">
            Delete
          </button>
        </div>
      </div>
    </Modal>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <p>© 2025 ResolveHub • Built for Frontend Wizards Stage 2</p>
      </div>
    </footer>
  </div>
</template>

<script>
import Toast from './Toast.vue'
import Modal from './Modal.vue'

export default {
  name: 'TicketManagement',
  components: { Toast, Modal },
  data() {
    return {
      user: null,
      tickets: [],
      isModalOpen: false,
      editingTicket: null,
      deleteConfirm: null,
      toast: null,
      formData: {
        title: '',
        description: '',
        status: 'open',
        priority: 'medium'
      },
      errors: {}
    }
  },
  mounted() {
    const session = localStorage.getItem('ticketapp_session')
    if (!session) {
      this.toast = { message: 'Your session has expired — please log in again.', type: 'error' }
      setTimeout(() => this.$emit('navigate', 'login'), 2000)
      return
    }
    const sessionData = JSON.parse(session)
    this.user = sessionData.user
    this.loadTickets()
  },
  methods: {
    loadTickets() {
      const storedTickets = localStorage.getItem('ticketapp_tickets')
      if (storedTickets) {
        this.tickets = JSON.parse(storedTickets)
      }
    },
    saveTickets(ticketsData) {
      localStorage.setItem('ticketapp_tickets', JSON.stringify(ticketsData))
      this.tickets = ticketsData
    },
    validateForm() {
      const newErrors = {}

      if (!this.formData.title || this.formData.title.trim() === '') {
        newErrors.title = 'Title is required'
      } else if (this.formData.title.length > 100) {
        newErrors.title = 'Title must be less than 100 characters'
      }

      if (!this.formData.status) {
        newErrors.status = 'Status is required'
      } else if (!['open', 'in_progress', 'closed'].includes(this.formData.status)) {
        newErrors.status = 'Status must be: open, in_progress, or closed'
      }

      if (this.formData.description && this.formData.description.length > 500) {
        newErrors.description = 'Description must be less than 500 characters'
      }

      this.errors = newErrors
      return Object.keys(newErrors).length === 0
    },
    clearError(field) {
      if (this.errors[field]) {
        delete this.errors[field]
      }
    },
    handleCreate() {
      this.formData = {
        title: '',
        description: '',
        status: 'open',
        priority: 'medium'
      }
      this.editingTicket = null
      this.errors = {}
      this.isModalOpen = true
    },
    handleEdit(ticket) {
      this.formData = {
        title: ticket.title,
        description: ticket.description || '',
        status: ticket.status,
        priority: ticket.priority || 'medium'
      }
      this.editingTicket = ticket
      this.errors = {}
      this.isModalOpen = true
    },
    handleSubmit() {
      if (!this.validateForm()) {
        this.toast = { message: 'Please fix the errors in the form', type: 'error' }
        return
      }

      if (this.editingTicket) {
        const updatedTickets = this.tickets.map(t =>
          t.id === this.editingTicket.id
            ? { ...t, ...this.formData, updatedAt: new Date().toISOString() }
            : t
        )
        this.saveTickets(updatedTickets)
        this.toast = { message: 'Ticket updated successfully!', type: 'success' }
      } else {
        const newTicket = {
          id: Date.now().toString(),
          ...this.formData,
          createdAt: new Date().toISOString(),
          updatedAt: new Date().toISOString()
        }
        this.saveTickets([...this.tickets, newTicket])
        this.toast = { message: 'Ticket created successfully!', type: 'success' }
      }

      this.isModalOpen = false
      this.formData = { title: '', description: '', status: 'open', priority: 'medium' }
      this.editingTicket = null
    },
    handleDelete(ticket) {
      this.deleteConfirm = ticket
    },
    confirmDelete() {
      if (this.deleteConfirm) {
        const updatedTickets = this.tickets.filter(t => t.id !== this.deleteConfirm.id)
        this.saveTickets(updatedTickets)
        this.toast = { message: 'Ticket deleted successfully!', type: 'success' }
        this.deleteConfirm = null
      }
    },
    getStatusColor(status) {
      switch(status) {
        case 'open':
          return { bg: 'rgba(16, 185, 129, 0.1)', border: '#10b981', text: '#10b981' }
        case 'in_progress':
          return { bg: 'rgba(245, 158, 11, 0.1)', border: '#f59e0b', text: '#f59e0b' }
        case 'closed':
          return { bg: 'rgba(148, 163, 184, 0.1)', border: '#94a3b8', text: '#94a3b8' }
        default:
          return { bg: 'rgba(4, 120, 87, 0.1)', border: '#047857', text: '#047857' }
      }
    },
    getStatusLabel(status) {
      switch(status) {
        case 'open': return 'Open'
        case 'in_progress': return 'In Progress'
        case 'closed': return 'Closed'
        default: return status
      }
    }
  }
}
</script>

<style scoped>
.ticket-management {
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
  border-bottom: 1px solid rgba(4, 120, 87, 0.1);
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

.nav-left {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.nav-brand {
  font-size: 1.75rem;
  font-weight: 700;
  background: linear-gradient(135deg, #047857, #10b981);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  cursor: pointer;
}

.btn-back {
  background: none;
  border: none;
  color: #94a3b8;
  font-size: 0.875rem;
  cursor: pointer;
  transition: color 0.3s ease;
}

.btn-back:hover {
  color: #047857;
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

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
  flex-wrap: wrap;
  gap: 1rem;
}

.header h1 {
  font-size: clamp(2rem, 4vw, 2.5rem);
  font-weight: 700;
}

.btn-create {
  padding: 1rem 2rem;
  background: linear-gradient(135deg, #047857, #059669);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(4, 120, 87, 0.4);
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.btn-create:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(4, 120, 87, 0.5);
}

.plus-icon {
  font-size: 1.25rem;
}

/* Empty State */
.empty-state {
  background: rgba(30, 41, 59, 0.6);
  border: 2px dashed rgba(4, 120, 87, 0.3);
  border-radius: 20px;
  padding: 4rem 2rem;
  text-align: center;
}

.empty-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.empty-state h3 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.empty-state p {
  color: #94a3b8;
  margin-bottom: 2rem;
}

/* Tickets Grid */
.tickets-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 1.5rem;
}

.ticket-card {
  background: rgba(30, 41, 59, 0.6);
  border: 1px solid rgba(4, 120, 87, 0.2);
  border-radius: 20px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
  cursor: pointer;
}

.ticket-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 32px rgba(4, 120, 87, 0.2);
}

.status-badge {
  display: inline-block;
  padding: 0.375rem 0.75rem;
  border: 1px solid;
  border-radius: 8px;
  font-size: 0.75rem;
  font-weight: 600;
  margin-bottom: 1rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.ticket-card h3 {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.75rem;
  color: #f1f5f9;
  line-height: 1.3;
}

.description {
  color: #94a3b8;
  font-size: 0.875rem;
  margin-bottom: 1rem;
  line-height: 1.6;
}

.priority {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.75rem;
  color: #cbd5e1;
  margin-bottom: 1rem;
}

.priority-value {
  padding: 0.25rem 0.5rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 4px;
  text-transform: capitalize;
}

.actions {
  display: flex;
  gap: 0.75rem;
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid rgba(4, 120, 87, 0.1);
}

.btn-edit {
  flex: 1;
  padding: 0.625rem;
  background: rgba(4, 120, 87, 0.1);
  color: #047857;
  border: 1px solid rgba(4, 120, 87, 0.3);
  border-radius: 8px;
  font-size: 0.875rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-edit:hover {
  background: rgba(4, 120, 87, 0.2);
}

.btn-delete {
  flex: 1;
  padding: 0.625rem;
  background: rgba(239, 68, 68, 0.1);
  color: #ef4444;
  border: 1px solid rgba(239, 68, 68, 0.3);
  border-radius: 8px;
  font-size: 0.875rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-delete:hover {
  background: rgba(239, 68, 68, 0.2);
}

/* Modal Content */
.modal-content {
  padding: 2rem;
}

.modal-content h2 {
  font-size: 1.75rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
  font-size: 0.875rem;
}

input,
textarea,
select {
  width: 100%;
  padding: 0.75rem;
  background: rgba(255, 255, 255, 0.05);
  border: 2px solid rgba(4, 120, 87, 0.2);
  border-radius: 10px;
  color: #f1f5f9;
  font-size: 1rem;
  font-family: inherit;
  transition: all 0.3s ease;
  outline: none;
}

input:focus,
textarea:focus,
select:focus {
  border-color: #047857;
  background: rgba(4, 120, 87, 0.05);
}

input.error,
textarea.error,
select.error {
  border-color: #ef4444;
}

textarea {
  resize: vertical;
  min-height: 100px;
}

select {
  cursor: pointer;
}

.error-message {
  color: #ef4444;
  font-size: 0.75rem;
  margin-top: 0.5rem;
  display: flex;
  align-items: center;
  gap: 0.25rem;
}

.modal-actions {
  display: flex;
  gap: 1rem;
  margin-top: 2rem;
}

.btn-cancel {
  flex: 1;
  padding: 0.875rem;
  background: rgba(255, 255, 255, 0.05);
  color: #cbd5e1;
  border: 1px solid rgba(4, 120, 87, 0.2);
  border-radius: 10px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-cancel:hover {
  background: rgba(255, 255, 255, 0.1);
}

.btn-submit {
  flex: 1;
  padding: 0.875rem;
  background: linear-gradient(135deg, #047857, #059669);
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(4, 120, 87, 0.4);
}

.btn-submit:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(4, 120, 87, 0.5);
}

/* Delete Modal */
.delete-modal {
  text-align: center;
}

.delete-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.delete-modal h2 {
  margin-bottom: 1rem;
}

.delete-modal p {
  color: #94a3b8;
  margin-bottom: 2rem;
}

.btn-delete-confirm {
  flex: 1;
  padding: 0.875rem;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(239, 68, 68, 0.4);
}

.btn-delete-confirm:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(239, 68, 68, 0.5);
}

/* Footer */
.footer {
  background: rgba(15, 23, 42, 0.8);
  border-top: 1px solid rgba(4, 120, 87, 0.1);
  padding: 2rem;
  margin-top: 4rem;
  text-align: center;
  color: #64748b;
  font-size: 0.875rem;
}
</style>