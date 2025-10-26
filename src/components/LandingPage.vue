<template>
  <div class="landing-page">
    <!-- Navigation -->
    <nav class="nav">
      <div class="nav-container">
        <div class="nav-brand">ResolveHub</div>
        <button class="btn-login" @click="$emit('navigate', 'login')">
          Login
        </button>
      </div>
    </nav>

    <!-- Hero Section -->
    <section class="hero">
      <!-- Decorative Circles -->
      <div class="circle circle-1"></div>
      <div class="circle circle-2"></div>

      <div class="hero-content">
        <div class="hero-text">
          <h1>Manage Tickets Like a Pro</h1>
          <p>
            Streamline your workflow with our powerful ticket management system.
            Track, organize, and resolve issues faster than ever before.
          </p>
          <div class="hero-buttons">
            <button class="btn btn-primary" @click="$emit('navigate', 'signup')">
              Get Started Free
            </button>
            <button class="btn btn-secondary" @click="$emit('navigate', 'login')">
              Sign In
            </button>
          </div>
        </div>
      </div>

      <!-- Wave SVG -->
      <svg class="wave" viewBox="0 0 1440 120" preserveAspectRatio="none">
        <path
          d="M0,64L48,69.3C96,75,192,85,288,80C384,75,480,53,576,48C672,43,768,53,864,58.7C960,64,1056,64,1152,58.7C1248,53,1344,43,1392,37.3L1440,32L1440,120L1392,120C1344,120,1248,120,1152,120C1056,120,960,120,864,120C768,120,672,120,576,120C480,120,384,120,288,120C192,120,96,120,48,120L0,120Z"
          fill="#0f172a"
        />
      </svg>
    </section>

    <!-- Features Section -->
    <section class="features">
      <div class="container">
        <h2>Everything You Need</h2>
        <div class="features-grid">
          <div v-for="feature in features" :key="feature.title" class="feature-card">
            <div class="feature-icon">{{ feature.icon }}</div>
            <h3>{{ feature.title }}</h3>
            <p>{{ feature.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section class="cta">
      <div class="circle circle-3"></div>
      <div class="container">
        <h2>Ready to Transform Your Workflow?</h2>
        <p>Join thousands of teams already managing their tickets efficiently</p>
        <button class="btn btn-primary" @click="$emit('navigate', 'signup')">
          Start Free Today
        </button>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <div class="footer-brand">ResolveHub</div>
        <p>Built with passion for Frontend Wizards Stage 2</p>
        <div class="footer-links">
          <span>© 2025 ResolveHub</span>
          <span>•</span>
          <span>All Rights Reserved</span>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'LandingPage',
  data() {
    return {
      features: [
        {
          icon: '🎯',
          title: 'Smart Organization',
          description: 'Organize tickets by status, priority, and category with intelligent tagging'
        },
        {
          icon: '⚡',
          title: 'Lightning Fast',
          description: 'Built for speed. Create, update, and manage tickets in milliseconds'
        },
        {
          icon: '🔒',
          title: 'Secure & Private',
          description: 'Your data is protected with enterprise-grade security standards'
        }
      ]
    }
  }
}
</script>

<style scoped>
.landing-page {
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
}

.nav-brand {
  font-size: 1.75rem;
  font-weight: 700;
  background: linear-gradient(135deg, #047857, #10b981);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.btn-login {
  padding: 0.75rem 2rem;
  background: linear-gradient(135deg, #047857, #059669);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(4, 120, 87, 0.3);
}

.btn-login:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(4, 120, 87, 0.4);
}

/* Hero Section */
.hero {
  position: relative;
  overflow: hidden;
  background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
}

.circle {
  position: absolute;
  border-radius: 50%;
  filter: blur(40px);
  z-index: 0;
}

.circle-1 {
  top: -100px;
  right: -100px;
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(4, 120, 87, 0.15), transparent);
}

.circle-2 {
  bottom: 10%;
  left: -50px;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(16, 185, 129, 0.15), transparent);
}

.circle-3 {
  top: 50%;
  right: -150px;
  transform: translateY(-50%);
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, rgba(5, 150, 105, 0.2), transparent);
  filter: blur(60px);
}

.hero-content {
  max-width: 1440px;
  margin: 0 auto;
  padding: 8rem 2rem 12rem;
  position: relative;
  z-index: 1;
}

.hero-text {
  text-align: center;
  max-width: 800px;
  margin: 0 auto;
}

.hero-text h1 {
  font-size: clamp(2.5rem, 6vw, 4.5rem);
  font-weight: 800;
  margin-bottom: 1.5rem;
  line-height: 1.1;
  background: linear-gradient(135deg, #f1f5f9, #cbd5e1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-text p {
  font-size: clamp(1.125rem, 2vw, 1.5rem);
  color: #94a3b8;
  margin-bottom: 3rem;
  line-height: 1.6;
}

.hero-buttons {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
}

.btn {
  padding: 1rem 2.5rem;
  border: none;
  border-radius: 12px;
  font-size: 1.125rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-primary {
  background: linear-gradient(135deg, #047857, #059669);
  color: white;
  box-shadow: 0 8px 24px rgba(4, 120, 87, 0.4);
}

.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 32px rgba(4, 120, 87, 0.5);
}

.btn-secondary {
  background: rgba(255, 255, 255, 0.05);
  color: #f1f5f9;
  border: 2px solid rgba(4, 120, 87, 0.3);
  backdrop-filter: blur(10px);
}

.btn-secondary:hover {
  border-color: rgba(4, 120, 87, 0.6);
  background: rgba(4, 120, 87, 0.1);
}

.wave {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: auto;
}

/* Features Section */
.features {
  background: #0f172a;
  padding: 6rem 2rem;
}

.container {
  max-width: 1440px;
  margin: 0 auto;
}

.features h2 {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 700;
  text-align: center;
  margin-bottom: 4rem;
  background: linear-gradient(135deg, #f1f5f9, #cbd5e1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.feature-card {
  background: rgba(30, 41, 59, 0.5);
  border: 1px solid rgba(4, 120, 87, 0.1);
  border-radius: 20px;
  padding: 2.5rem;
  transition: all 0.3s ease;
  cursor: pointer;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
}

.feature-card:hover {
  transform: translateY(-8px);
  border-color: rgba(4, 120, 87, 0.3);
  box-shadow: 0 12px 32px rgba(4, 120, 87, 0.2);
}

.feature-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.feature-card h3 {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #f1f5f9;
}

.feature-card p {
  color: #94a3b8;
  line-height: 1.6;
}

/* CTA Section */
.cta {
  background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
  padding: 6rem 2rem;
  position: relative;
  overflow: hidden;
  text-align: center;
}

.cta h2 {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 700;
  margin-bottom: 1.5rem;
  background: linear-gradient(135deg, #f1f5f9, #cbd5e1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.cta p {
  font-size: 1.25rem;
  color: #94a3b8;
  margin-bottom: 2.5rem;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

/* Footer */
.footer {
  background: #0f172a;
  border-top: 1px solid rgba(4, 120, 87, 0.1);
  padding: 3rem 2rem;
  text-align: center;
}

.footer-brand {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  background: linear-gradient(135deg, #047857, #10b981);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.footer p {
  color: #64748b;
  margin-bottom: 1.5rem;
}

.footer-links {
  display: flex;
  justify-content: center;
  gap: 2rem;
  flex-wrap: wrap;
  color: #94a3b8;
  font-size: 0.875rem;
}
</style>