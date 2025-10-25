<template>
  <div v-if="show" class="toast" :class="type">
    <div class="toast-content">
      <span class="toast-icon">{{ type === 'error' ? '⚠️' : '✓' }}</span>
      <span>{{ message }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Toast',
  props: {
    message: String,
    type: {
      type: String,
      default: 'success'
    }
  },
  data() {
    return {
      show: true
    }
  },
  mounted() {
    setTimeout(() => {
      this.show = false
      this.$emit('close')
    }, 3000)
  }
}
</script>

<style scoped>
.toast {
  position: fixed;
  top: 2rem;
  right: 2rem;
  padding: 1rem 1.5rem;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
  z-index: 9999;
  animation: slideIn 0.3s ease-out;
  max-width: 400px;
}

.toast.success {
  background: #10b981;
  color: white;
}

.toast.error {
  background: #ef4444;
  color: white;
}

.toast-content {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.toast-icon {
  font-size: 1.25rem;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}
</style>