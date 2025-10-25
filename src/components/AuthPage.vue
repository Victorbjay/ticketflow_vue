<template>
  <div class="auth-page">
    <!-- Background Decorations -->
    <div class="circle circle-1"></div>
    <div class="circle circle-2"></div>

    <!-- Toast -->
    <Toast v-if="toast" :message="toast.message" :type="toast.type" @close="toast = null" />

    <div class="auth-container">
      <!-- Back to Home -->
      <div class="back-link">
        <button @click="$emit('navigate', 'home')" class="btn-back">
          ← Back to Home
        </button>
      </div>

      <div class="auth-card">
        <!-- Logo/Title -->
        <div class="auth-header">
          <div class="auth-brand">TicketFlow</div>
          <h2>{{ isLogin ? 'Welcome Back' : 'Create Account' }}</h2>
          <p>{{ isLogin ? 'Sign in to manage your tickets' : 'Sign up to get started' }}</p>
        </div>

        <!-- Form -->
        <form @submit.prevent="handleSubmit">
          <!-- Email -->
          <div class="form-group">
            <label for="email">Email Address</label>
            <input
              type="email"
              id="email"
              v-model="formData.email"
              placeholder="you@example.com"
              :class="{ error: errors.email }"
              @input="clearError('email')"
            />
            <div v-if="errors.email" class="error-message">
              <span>⚠</span> {{ errors.email }}
            </div>
          </div>

          <!-- Password -->
          <div class="form-group">
            <label for="password">Password</label>
            <input
              type="password"
              id="password"
              v-model="formData.password"
              placeholder="••••••••"
              :class="{ error: errors.password }"
              @input="clearError('password')"
            />
            <div v-if="errors.password" class="error-message">
              <span>⚠</span> {{ errors.password }}
            </div>
          </div>

          <!-- Confirm Password (Signup only) -->
          <div v-if="!isLogin" class="form-group">
            <label for="confirmPassword">Confirm Password</label>
            <input
              type="password"
              id="confirmPassword"
              v-model="formData.confirmPassword"
              placeholder="••••••••"
              :class="{ error: errors.confirmPassword }"
              @input="clearError('confirmPassword')"
            />
            <div v-if="errors.confirmPassword" class="error-message">
              <span>⚠</span> {{ errors.confirmPassword }}
            </div>
          </div>

          <!-- Submit Button -->
          <button type="submit" class="btn-submit">
            {{ isLogin ? 'Sign In' : 'Create Account' }}
          </button>
        </form>

        <!-- Toggle Login/Signup -->
        <div class="auth-toggle">
          <p>
            {{ isLogin ? "Don't have an account?" : "Already have an account?" }}
            <button @click="toggleMode" class="btn-toggle">
              {{ isLogin ? 'Sign up' : 'Sign in' }}
            </button>
          </p>
        </div>

        <!-- Demo Credentials -->
        <div v-if="isLogin" class="demo-box">
          <p class="demo-title">Demo Credentials:</p>
          <p>Email: demo@ticketflow.com</p>
          <p>Password: demo123</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Toast from './Toast.vue'

export default {
  name: 'AuthPage',
  components: { Toast },
  props: {
    isLogin: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      formData: {
        email: '',
        password: '',
        confirmPassword: ''
      },
      errors: {},
      toast: null
    }
  },
  methods: {
    validateEmail(email) {
      return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
    },
    validateForm() {
      const newErrors = {}

      if (!this.formData.email) {
        newErrors.email = 'Email is required'
      } else if (!this.validateEmail(this.formData.email)) {
        newErrors.email = 'Please enter a valid email address'
      }

      if (!this.formData.password) {
        newErrors.password = 'Password is required'
      } else if (this.formData.password.length < 6) {
        newErrors.password = 'Password must be at least 6 characters'
      }

      if (!this.isLogin) {
        if (!this.formData.confirmPassword) {
          newErrors.confirmPassword = 'Please confirm your password'
        } else if (this.formData.password !== this.formData.confirmPassword) {
          newErrors.confirmPassword = 'Passwords do not match'
        }
      }

      this.errors = newErrors
      return Object.keys(newErrors).length === 0
    },
    clearError(field) {
      if (this.errors[field]) {
        delete this.errors[field]
      }
    },
    handleSubmit() {
      if (!this.validateForm()) {
        this.toast = { message: 'Please fix the errors in the form', type: 'error' }
        return
      }

      if (this.isLogin) {
        const storedUser = localStorage.getItem('ticketapp_user')
        if (storedUser) {
          const user = JSON.parse(storedUser)
          if (user.email === this.formData.email && user.password === this.formData.password) {
            localStorage.setItem('ticketapp_session', JSON.stringify({
              token: 'mock-jwt-token-' + Date.now(),
              user: { email: user.email },
              timestamp: Date.now()
            }))
            this.toast = { message: 'Login successful! Redirecting...', type: 'success' }
            setTimeout(() => this.$emit('auth'), 1500)
            return
          }
        }
        this.toast = { message: 'Invalid credentials. Please try again.', type: 'error' }
      } else {
        localStorage.setItem('ticketapp_user', JSON.stringify({
          email: this.formData.email,
          password: this.formData.password
        }))
        localStorage.setItem('ticketapp_session', JSON.stringify({
          token: 'mock-jwt-token-' + Date.now(),
          user: { email: this.formData.email },
          timestamp: Date.now()
        }))
        this.toast = { message: 'Account created successfully! Redirecting...', type: 'success' }
        setTimeout(() => this.$emit('auth'), 1500)
      }
    },
    toggleMode() {
      this.$emit('navigate', this.isLogin ? 'signup' : 'login')
      this.formData = { email: '', password: '', confirmPassword: '' }
      this.errors = {}
    }
  }
}
</script>

<style scoped>
.auth-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
  color: #f1f5f9;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  position: relative;
  overflow: hidden;
}

.circle {
  position: absolute;
  border-radius: 50%;
  filter: blur(60px);
}

.circle-1 {
  top: -100px;
  left: -100px;
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(4, 120, 87, 0.15), transparent);
}

.circle-2 {
  bottom: -100px;
  right: -100px;
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(16, 185, 129, 0.15), transparent);
}

.auth-container {
  width: 100%;
  max-width: 480px;
  position: relative;
  z-index: 1;
}

.back-link {
  margin-bottom: 2rem;
  text-align: center;
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

.auth-card {
  background: rgba(30, 41, 59, 0.8);
  backdrop-filter: blur(20px);
  border-radius: 24px;
  padding: 3rem;
  border: 1px solid rgba(4, 120, 87, 0.2);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

.auth-header {
  text-align: center;
  margin-bottom: 2.5rem;
}

.auth-brand {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  background: linear-gradient(135deg, #047857, #10b981);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.auth-header h2 {
  font-size: 1.5rem;
  font-weight: 600;
  color: #f1f5f9;
  margin-bottom: 0.5rem;
}

.auth-header p {
  color: #94a3b8;
  font-size: 0.875rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  color: #cbd5e1;
  font-weight: 500;
  font-size: 0.875rem;
}

input {
  width: 100%;
  padding: 0.875rem 1rem;
  background: rgba(255, 255, 255, 0.05);
  border: 2px solid rgba(4, 120, 87, 0.2);
  border-radius: 10px;
  color: #f1f5f9;
  font-size: 1rem;
  transition: all 0.3s ease;
  outline: none;
}

input:focus {
  border-color: #047857;
  background: rgba(4, 120, 87, 0.05);
}

input.error {
  border-color: #ef4444;
}

.error-message {
  color: #ef4444;
  font-size: 0.75rem;
  margin-top: 0.5rem;
  display: flex;
  align-items: center;
  gap: 0.25rem;
}

.btn-submit {
  width: 100%;
  padding: 1rem;
  background: linear-gradient(135deg, #047857, #059669);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(4, 120, 87, 0.4);
  margin-top: 0.5rem;
}

.btn-submit:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(4, 120, 87, 0.5);
}

.auth-toggle {
  text-align: center;
  margin-top: 2rem;
  padding-top: 2rem;
  border-top: 1px solid rgba(4, 120, 87, 0.1);
}

.auth-toggle p {
  color: #94a3b8;
  font-size: 0.875rem;
}

.btn-toggle {
  background: none;
  border: none;
  color: #047857;
  font-weight: 600;
  cursor: pointer;
  font-size: 0.875rem;
  text-decoration: underline;
  margin-left: 0.25rem;
}

.demo-box {
  margin-top: 1.5rem;
  padding: 1rem;
  background: rgba(4, 120, 87, 0.1);
  border-radius: 10px;
  border: 1px solid rgba(4, 120, 87, 0.2);
}

.demo-title {
  font-size: 0.75rem;
  color: #cbd5e1;
  margin-bottom: 0.5rem;
  font-weight: 600;
}

.demo-box p {
  font-size: 0.75rem;
  color: #94a3b8;
  margin: 0.25rem 0;
}
</style>