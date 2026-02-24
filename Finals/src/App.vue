<script>
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  'https://hjtozrgbkorqwabyoqlv.supabase.co',
  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImhqdG96cmdia29ycXdhYnlvcWx2Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzE4NDYzMDUsImV4cCI6MjA4NzQyMjMwNX0.bF17wqKB0k05g_cMNLpSWXjhWMGgne-ZObyIiRU8_lo'
)

export default {
  data() {
    return {
      guestbook: [],
      name: '',
      message: '',
      loading: false,
      isDark: false
    }
  },
  methods: {
    toggleTheme() {
      this.isDark = !this.isDark;
      document.documentElement.setAttribute('data-theme', this.isDark ? 'dark' : 'light');
      localStorage.setItem('theme', this.isDark ? 'dark' : 'light');
    },
    async fetchGuestbook() {
      const { data, error } = await supabase
        .from('guestbook')
        .select('*')
        .order('created_at', { ascending: false })
      if (!error) this.guestbook = data
    },
    async submitMessage() {
      if (!this.name || !this.message) return
      this.loading = true
      const { error } = await supabase
        .from('guestbook')
        .insert([{ name: this.name, message: this.message }])
      if (!error) {
        this.name = ''
        this.message = ''
        await this.fetchGuestbook()
      }
      this.loading = false
    }
  },
  mounted() {
    this.fetchGuestbook();
    // Check for saved theme
    const savedTheme = localStorage.getItem('theme');
    if (savedTheme === 'dark') {
      this.isDark = true;
      document.documentElement.setAttribute('data-theme', 'dark');
    }
  }
}
</script>

<template>
  <div class="app-wrapper">
    <!-- GOOGLE STYLE NAV -->
    <nav class="google-nav">
      <div class="nav-container">
        <a href="#" class="nav-item">Home</a>
        <a href="#about" class="nav-item">About</a>
        <a href="#work" class="nav-item">Gallery</a>
        <a href="#guestbook" class="nav-item">Guestbook</a>
        
        <!-- CUTE THEME TOGGLE -->
        <button @click="toggleTheme" class="theme-toggle">
          <span v-if="!isDark" class="icon-pop">☀️</span>
          <span v-else class="icon-pop">🌙</span>
        </button>
      </div>
    </nav>

   <!-- HERO SECTION -->
      <header class="hero">
        <div class="hero-flex-container">
          <!-- Profile Picture Box -->
          <div class="profile-frame">
            <img src="/images/6.jpg" alt="Terrence Josh" class="profile-img" />
          </div>

          <!-- Text Content Box -->
          <div class="hero-text-content">
            <h1 class="name-title">Terrence Josh Q. Corpuz</h1>
            <p class="description">
              Hi! I’m a second-year Computer Science student specializing in 
              <strong>Cybersecurity and Forensics</strong> at Asia Pacific College. 
              I’m passionate about web development, design, and building clean, 
              functional digital experiences. I like playing Volleyball and any physical activities.
            </p>

            <!-- NEW SOCIAL LINKS -->
            <div class="social-links-row">
              <a href="https://www.facebook.com/terrence.josh.corpuz.2024/" target="_blank" class="social-icon fb">
                <i class="fa-brands fa-facebook"></i>
              </a>
              <a href="https://www.linkedin.com/in/terrence-corpuz-corpuz-a3860a342/" target="_blank" class="social-icon li">
                <i class="fa-brands fa-linkedin"></i>
              </a>
              <a href="https://www.instagram.com/terrencejuice/" target="_blank" class="social-icon ig">
                <i class="fa-brands fa-instagram"></i>
              </a>
              <a href="https://mail.google.com/mail/u/0/?pli=1#inbox?compose=new" class="social-icon gm">
                <i class="fa-solid fa-envelope"></i>
              </a>
            </div>
          </div> <!-- Closes hero-text-content -->
        </div> <!-- Closes hero-flex-container -->
      </header> <!-- Closes hero -->

      <!-- INFO GRID -->
      <section id="about" class="info-grid">
        <div class="bento-card">
          <div class="card-header"><h3>Skills</h3></div>
          <div class="tags">
            <span>JavaScript</span><span>Supabase</span><span>Tailwind</span>
            <span>Html/CSS</span><span>Python</span><span>Basic SQL</span><span>XAMMP</span><span>Cisco Packet</span>
          </div>
        </div>
        <div class="bento-card">
          <div class="card-header"><h3>Interests</h3></div>
          <ul class="interest-list">
            <li>Web Development</li><li>Minimal Design</li><li>Technology</li><li>Volleyball</li>
          </ul>
        </div>
      </section>

      <!-- GALLERY -->
      <section id="work" class="gallery-section">
        <h2 class="section-label">Gallery / Visuals</h2>
        <div class="slider">
          <div class="slide-track">
            <div class="slide"><img src="/images/1.jpg" /></div>
            <div class="slide"><img src="/images/2.JPG" /></div>
            <div class="slide"><img src="/images/3.jpg" /></div>
            <div class="slide"><img src="/images/4.jpg" /></div>
            <div class="slide"><img src="/images/5.jpg" /></div>
            <div class="slide"><img src="/images/6.jpg" /></div>
            <div class="slide"><img src="/images/7.jpg" /></div>
            <div class="slide"><img src="/images/8.jpg" /></div>
            <!-- Duplicates -->
            <div class="slide"><img src="/images/1.jpg" /></div>
            <div class="slide"><img src="/images/2.JPG" /></div>
          </div>
        </div>
      </section>

      <!-- CLEAN GUESTBOOK SECTION -->
      <section id="guestbook" class="guestbook-section">
        <h2 class="section-label">Guestbook</h2>
        
        <div class="guestbook-clean-layout">
          <!-- Form Side -->
          <div class="form-container">
            <h3>Leave a note</h3>
            <p>Your thoughts on the page.</p>
            <div class="input-group">
              <input v-model="name" placeholder="Your Name" />
              <textarea v-model="message" placeholder="Write a message..." rows="3"></textarea>
              <button @click="submitMessage" :disabled="loading" class="submit-pill">
                {{ loading ? "Sending..." : "Post Message" }}
              </button>
            </div>
          </div>

          <!-- Messages Side -->
          <div class="messages-stack">
            <div v-for="entry in guestbook" :key="entry.id" class="ledger-entry">
              <div class="ledger-header">
                <span class="ledger-name">{{ entry.name }}</span>
                <span class="ledger-date">{{ new Date(entry.created_at).toLocaleDateString() }}</span>
              </div>
              <p class="ledger-text">{{ entry.message }}</p>
            </div>
          </div>
        </div>
      </section>
    

    
      <footer class="footer-section"> 
       <p>© 2026 Terrence Josh Q. Corpuz</p>
      <div class="footer-links">
        <span class="footer-label">Learning Resources & Tools:</span>
        <div class="reference-grid">
          <a href="https://aistudio.google.com/" target="_blank" class="ref-chip">Google Studio AI</a>
          <a href="https://www.w3schools.com/" target="_blank" class="ref-chip">W3Schools</a>
          <a href="https://supabase.com/docs/reference/javascript/introduction" target="_blank" class="ref-chip">Supabase API</a>
          <a href="https://nextjs.org/docs" target="_blank" class="ref-chip">Next.js Docs</a>
        </div>
      </div>
      
      <p class="copyright">© 2026 Terrence Josh Q. Corpuz • Built with Vue & Supabase</p>
    </footer>
  </div>
</template>