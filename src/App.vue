<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'

const scrolled = ref(false)
const menuOpen = ref(false)
const activeCategory = ref('All')
const selectedCar = ref(null)
const categories = ['All', 'MPV', 'SUV', 'Sedan', 'Premium']

const cars = ref([
  { id: 1, name: 'Toyota Innova Zenix', category: 'MPV', price: 750000, image: 'https://images.unsplash.com/photo-1619767886558-efdc259cde1a?auto=format&fit=crop&w=1200&q=85', seats: 7, transmission: 'Automatic', fuel: 'Hybrid', badge: 'Most Popular', description: 'Kabin luas, nyaman, dan ideal untuk perjalanan keluarga maupun bisnis.' },
  { id: 2, name: 'Toyota Fortuner', category: 'SUV', price: 950000, image: 'https://images.unsplash.com/photo-1552519507-da3b142c6e3d?auto=format&fit=crop&w=1200&q=85', seats: 7, transmission: 'Automatic', fuel: 'Diesel', badge: 'Executive', description: 'SUV tangguh dengan kabin premium untuk perjalanan jauh yang nyaman.' },
  { id: 3, name: 'Honda Civic RS', category: 'Sedan', price: 850000, image: 'https://images.unsplash.com/photo-1606664515524-ed2f786a0bd6?auto=format&fit=crop&w=1200&q=85', seats: 5, transmission: 'Automatic', fuel: 'Petrol', badge: 'Sporty', description: 'Sedan stylish dengan handling responsif untuk kebutuhan profesional.' },
  { id: 4, name: 'Toyota Alphard', category: 'Premium', price: 2200000, image: 'https://images.unsplash.com/photo-1563720223185-11003d516935?auto=format&fit=crop&w=1200&q=85', seats: 6, transmission: 'Automatic', fuel: 'Petrol', badge: 'Luxury', description: 'Pengalaman perjalanan kelas premium dengan captain seat yang nyaman.' },
  { id: 5, name: 'Honda BR-V', category: 'SUV', price: 600000, image: 'https://images.unsplash.com/photo-1542362567-b07e54358753?auto=format&fit=crop&w=1200&q=85', seats: 7, transmission: 'Automatic', fuel: 'Petrol', badge: 'Value Pick', description: 'Praktis dan efisien untuk mobilitas harian bersama keluarga.' },
  { id: 6, name: 'Toyota Camry', category: 'Sedan', price: 1250000, image: 'https://images.unsplash.com/photo-1621007947382-bb3c3994e3fb?auto=format&fit=crop&w=1200&q=85', seats: 5, transmission: 'Automatic', fuel: 'Hybrid', badge: 'Business', description: 'Sedan executive dengan kenyamanan dan kesan profesional yang kuat.' }
])

const filteredCars = computed(() => activeCategory.value === 'All' ? cars.value : cars.value.filter(c => c.category === activeCategory.value))
const formatPrice = value => new Intl.NumberFormat('id-ID').format(value)

const isLoggedIn = ref(false)
const showLoginModal = ref(false)
const loginUsername = ref('')
const loginPassword = ref('')
const loginError = ref(false)
const editingCar = ref(null)

const handleLogin = () => {
  if (loginUsername.value === 'admin' && loginPassword.value === 'admin') {
    isLoggedIn.value = true
    showLoginModal.value = false
    loginError.value = false
    loginUsername.value = ''
    loginPassword.value = ''
  } else {
    loginError.value = true
  }
}

const handleLogout = () => {
  isLoggedIn.value = false
}

const openEditModal = (car) => {
  editingCar.value = { ...car }
}

const saveEdit = () => {
  const index = cars.value.findIndex(c => c.id === editingCar.value.id)
  if (index !== -1) {
    cars.value[index] = { ...editingCar.value }
  }
  editingCar.value = null
}

const whatsapp = (car = null) => {
  const text = car ? `Halo DRIVEGO, saya tertarik menyewa ${car.name}. Mohon info ketersediaan dan harga.` : 'Halo DRIVEGO, saya ingin bertanya mengenai rental mobil.'
  window.open(`https://wa.me/6281234567890?text=${encodeURIComponent(text)}`, '_blank')
}
const scrollTo = id => { document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' }); menuOpen.value = false }
const onScroll = () => { scrolled.value = window.scrollY > 30 }
onMounted(() => window.addEventListener('scroll', onScroll))
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<template>
  <div class="site-shell">
    <header :class="['navbar', { scrolled }]">
      <a class="brand" href="#" @click.prevent="scrollTo('home')"><span class="brand-mark">D</span><span>DRIVE<span class="muted-brand">GO</span></span></a>
      <nav :class="['nav-links', { open: menuOpen }]">
        <a href="#home" @click="menuOpen=false">Home</a><a href="#fleet" @click="menuOpen=false">Fleet</a><a href="#services" @click="menuOpen=false">Services</a><a href="#about" @click="menuOpen=false">About</a>
        <button class="mobile-cta" @click="whatsapp()">Book Now</button>
      </nav>
      <button class="nav-cta" @click="whatsapp()">Book a Car <span>↗</span></button>
      <button class="menu-btn" @click="menuOpen=!menuOpen" aria-label="Menu"><span></span><span></span></button>
    </header>

    <main>
      <section id="home" class="hero">
        <div class="hero-bg"></div><div class="hero-overlay"></div>
        <div class="container hero-content">
          <div class="hero-copy">
            <div class="eyebrow light"><span></span> PREMIUM CAR RENTAL</div>
            <h1>Your journey.<br><em>Our vehicle.</em></h1>
            <p>Experience comfort, reliability, and freedom with a carefully selected fleet built for every kind of journey.</p>
            <div class="hero-actions"><button class="btn btn-light" @click="scrollTo('fleet')">Explore Fleet <span>→</span></button><button class="text-btn" @click="scrollTo('about')">Discover DRIVEGO <span>↗</span></button></div>
          </div>
          <div class="hero-card"><div class="hero-card-top"><span>FEATURED</span><span>01 / 06</span></div><div class="hero-car-name">Toyota<br><strong>Innova Zenix</strong></div><div class="hero-specs"><span>7 Seats</span><i></i><span>Automatic</span><i></i><span>Hybrid</span></div><div class="hero-price">From <strong>Rp 750K</strong> <small>/ day</small></div></div>
        </div>
        <div class="scroll-indicator"><span></span> SCROLL TO EXPLORE</div>
      </section>

      <section class="stats"><div class="container stats-grid"><div><strong>500<span>+</span></strong><small>Happy Customers</small></div><div><strong>50<span>+</span></strong><small>Premium Vehicles</small></div><div><strong>5<span>+</span></strong><small>Years Experience</small></div><div><strong>24<span>/7</span></strong><small>Customer Support</small></div></div></section>

      <section id="fleet" class="section fleet-section"><div class="container"><div class="section-head"><div><div class="eyebrow"><span></span> OUR FLEET</div><h2>Find your perfect <em>ride.</em></h2></div><p>From practical city cars to executive luxury, choose the vehicle that fits your journey.</p></div><div class="filters"><button v-for="category in categories" :key="category" :class="{ active: activeCategory === category }" @click="activeCategory=category">{{ category }}</button></div><div class="car-grid"><article v-for="car in filteredCars" :key="car.id" class="car-card"><div class="car-image"><img :src="car.image" :alt="car.name" loading="lazy"><span class="car-badge">{{ car.badge }}</span><button class="round-btn" @click="selectedCar=car">↗</button></div><div class="car-info"><div class="car-meta"><span>{{ car.category }}</span><span>•</span><span>{{ car.seats }} Seats</span></div><h3>{{ car.name }}</h3><div class="car-bottom"><div><small>Starting from</small><strong>Rp {{ formatPrice(car.price) }} <i>/ day</i></strong></div><button class="arrow-btn" @click="selectedCar=car">→</button></div><button v-if="isLoggedIn" class="btn btn-dark" style="margin-top: 15px; width: 100%; font-size: 0.9rem; padding: 0.5rem;" @click.stop="openEditModal(car)">Edit Data</button></div></article></div></div></section>

      <section id="services" class="section services-section"><div class="container"><div class="section-head centered"><div><div class="eyebrow"><span></span> WHAT WE OFFER</div><h2>More than just a <em>rental.</em></h2></div><p>Everything you need to make your journey smooth, comfortable, and memorable.</p></div><div class="service-grid"><article><span class="service-no">01</span><div class="service-icon">◈</div><h3>Self Drive</h3><p>Drive on your own terms with flexible rental options and well-maintained vehicles.</p><a href="#" @click.prevent="whatsapp()">Learn more ↗</a></article><article><span class="service-no">02</span><div class="service-icon">✦</div><h3>With Driver</h3><p>Relax and enjoy the journey with our professional, experienced drivers.</p><a href="#" @click.prevent="whatsapp()">Learn more ↗</a></article><article><span class="service-no">03</span><div class="service-icon">⌁</div><h3>Airport Transfer</h3><p>On-time airport pickup and drop-off for a seamless start and finish.</p><a href="#" @click.prevent="whatsapp()">Learn more ↗</a></article><article><span class="service-no">04</span><div class="service-icon">□</div><h3>Corporate Rental</h3><p>Tailored mobility solutions for businesses, teams, and executive travel.</p><a href="#" @click.prevent="whatsapp()">Learn more ↗</a></article></div></div></section>

      <section id="about" class="section about-section"><div class="container about-grid"><div class="about-image"><img src="https://images.unsplash.com/photo-1503376780353-7e6692767b70?auto=format&fit=crop&w=1400&q=85" alt="Premium car" loading="lazy"><div class="experience"><strong>5+</strong><span>Years of<br>experience</span></div></div><div class="about-copy"><div class="eyebrow"><span></span> ABOUT DRIVEGO</div><h2>We believe every journey <em>matters.</em></h2><p>DRIVEGO was built around one simple idea: renting a car should feel effortless. We combine a curated fleet, transparent service, and people who genuinely care about your journey.</p><p>Whether you're heading to a business meeting, a weekend escape, or a special occasion, we're here to make every kilometer count.</p><div class="about-points"><span>✓ Well-maintained fleet</span><span>✓ Transparent pricing</span><span>✓ Fast & friendly support</span><span>✓ Flexible rental plans</span></div><button class="btn btn-dark" @click="whatsapp()">Talk to our team <span>↗</span></button></div></div></section>

      <section class="testimonial"><div class="container"><div class="quote-mark">“</div><blockquote>Everything was seamless from booking to returning the car. The Innova was spotless and the team was incredibly helpful. <em>Highly recommended.</em></blockquote><div class="customer"><div class="avatar">AR</div><div><strong>Andi Rahman</strong><span>Jakarta · Business Traveller</span></div><div class="stars">★★★★★</div></div></div></section>

      <section class="cta-section"><div class="container cta-inner"><div><div class="eyebrow light"><span></span> READY TO GO?</div><h2>Your next journey<br>starts <em>here.</em></h2></div><button class="btn btn-light" @click="whatsapp()">Book your ride <span>↗</span></button></div></section>
    </main>

    <footer><div class="container footer-top"><div><a class="brand footer-brand" href="#" @click.prevent="scrollTo('home')"><span class="brand-mark">D</span><span>DRIVE<span class="muted-brand">GO</span></span></a><p>Premium mobility for every journey.</p></div><div class="footer-col"><h4>Explore</h4><a href="#fleet">Our Fleet</a><a href="#services">Services</a><a href="#about">About Us</a></div><div class="footer-col"><h4>Contact</h4><a href="tel:+6281234567890">+62 812 3456 7890</a><a href="mailto:hello@drivego.id">hello@drivego.id</a><span>Jakarta, Indonesia</span></div><div class="footer-col"><h4>Follow</h4><a href="#">Instagram ↗</a><a href="#">TikTok ↗</a><h4 style="margin-top: 1rem;">Admin</h4><a v-if="!isLoggedIn" href="#" @click.prevent="showLoginModal=true">Login</a><a v-else href="#" @click.prevent="handleLogout">Logout</a></div></div><div class="container footer-bottom"><span>© 2026 DRIVEGO. All rights reserved.</span><span>Privacy · Terms</span></div></footer>

    <button class="floating-wa" @click="whatsapp()" aria-label="WhatsApp">◔<span>Chat with us</span></button>

    <div v-if="selectedCar" class="modal-backdrop" @click.self="selectedCar=null"><div class="modal"><button class="modal-close" @click="selectedCar=null">×</button><img :src="selectedCar.image" :alt="selectedCar.name"><div class="modal-body"><div class="car-meta">{{ selectedCar.category }} · {{ selectedCar.badge }}</div><h2>{{ selectedCar.name }}</h2><p>{{ selectedCar.description }}</p><div class="modal-specs"><span>👤 {{ selectedCar.seats }} Seats</span><span>⚙ {{ selectedCar.transmission }}</span><span>◉ {{ selectedCar.fuel }}</span></div><div class="modal-action"><strong>Rp {{ formatPrice(selectedCar.price) }} <small>/ day</small></strong><button class="btn btn-dark" @click="whatsapp(selectedCar)">Book this car ↗</button></div></div></div></div>

    <!-- Login Modal -->
    <div v-if="showLoginModal" class="modal-backdrop" @click.self="showLoginModal=false">
      <div class="modal" style="max-width: 400px; padding: 2rem;">
        <button class="modal-close" @click="showLoginModal=false">×</button>
        <h2>Admin Login</h2>
        <form @submit.prevent="handleLogin" style="display: flex; flex-direction: column; gap: 1rem; margin-top: 1rem;">
          <input type="text" v-model="loginUsername" placeholder="Username" style="padding: 0.5rem; border: 1px solid #ccc; border-radius: 4px; color: #333; background: #fff;">
          <input type="password" v-model="loginPassword" placeholder="Password" style="padding: 0.5rem; border: 1px solid #ccc; border-radius: 4px; color: #333; background: #fff;">
          <p v-if="loginError" style="color: #ef4444; margin: 0; font-size: 0.9rem;">Username/password salah! (hint: admin/admin)</p>
          <button type="submit" class="btn btn-dark" style="margin-top: 0.5rem;">Login</button>
        </form>
      </div>
    </div>

    <!-- Edit Modal -->
    <div v-if="editingCar" class="modal-backdrop" @click.self="editingCar=null">
      <div class="modal" style="max-width: 500px; padding: 2rem; max-height: 90vh; overflow-y: auto;">
        <button class="modal-close" @click="editingCar=null">×</button>
        <h2>Edit {{ editingCar.name }}</h2>
        <form @submit.prevent="saveEdit" style="display: flex; flex-direction: column; gap: 1rem; margin-top: 1rem; text-align: left;">
          <div>
            <label style="display: block; font-weight: bold; margin-bottom: 0.5rem;">URL Foto / Image</label>
            <input type="text" v-model="editingCar.image" style="width: 100%; padding: 0.5rem; border: 1px solid #ccc; border-radius: 4px; color: #333; background: #fff; box-sizing: border-box;">
          </div>
          <div>
            <label style="display: block; font-weight: bold; margin-bottom: 0.5rem;">Deskripsi Singkat</label>
            <textarea v-model="editingCar.description" rows="4" style="width: 100%; padding: 0.5rem; border: 1px solid #ccc; border-radius: 4px; color: #333; background: #fff; box-sizing: border-box; resize: vertical;"></textarea>
          </div>
          <button type="submit" class="btn btn-dark" style="margin-top: 1rem;">Simpan Perubahan</button>
        </form>
      </div>
    </div>
  </div>
</template>
