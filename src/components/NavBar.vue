<template>
  <header class="njn-navbar">
    <div class="njn-logo ml-4"><img src="../assets/Logos/Logo White.svg"></div>
    <nav class="njn-nav-links desktop">
      <a href="/">Home</a>
      <a href="/about">About</a>
      <a href="/services">Services</a>
      <a href="/contact">Contact</a>
    </nav>

    <!-- Hamburger Menu -->
    <button class="njn-hamburger" @click="toggleSidebar">&#9776;</button>

    <!-- Mobile Sidebar -->
    <div class="njn-sidebar" :class="{ open: isSidebarOpen }">
      <button class="njn-close-btn" @click="toggleSidebar">&times;</button>
      <a href="/" @click="toggleSidebar">Home</a>
      <a href="/about" @click="toggleSidebar">About</a>
      <a href="/services" @click="toggleSidebar">Services</a>
      <a href="/contact" @click="toggleSidebar">Contact</a>
    </div>

    <!-- Overlay -->
    <div class="njn-overlay" v-if="isSidebarOpen" @click="toggleSidebar"></div>
  </header>
</template>

<script setup>
import { ref } from 'vue'
const isSidebarOpen = ref(false)

const toggleSidebar = () => {
  isSidebarOpen.value = !isSidebarOpen.value
}
</script>

<style scoped>
/* Top Navbar */
.njn-navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #000000;
  padding: 1rem 2rem;
  position: sticky;
  color: white;
  z-index: 1000;
  top: 0;
}

.njn-logo {
  font-size: 1.5rem;
  font-weight: bold;
}

.njn-nav-links a {
  color: white;
  margin-left: 1.5rem;
  text-decoration: none;
  font-weight: 500;
}

.njn-nav-links a:hover {
  text-decoration: underline;
}

/* Hide on mobile */
.desktop {
  display: flex;
}

.njn-hamburger {
  font-size: 1.5rem;
  background: none;
  color: white;
  border: none;
  display: none;
  cursor: pointer;
}

/* Sidebar */
.njn-sidebar {
  position: fixed;
  top: 0;
  right: -250px;
  width: 250px;
  height: 100%;
  background-color: #34495e;
  color: white;
  padding: 2rem 1rem;
  display: flex;
  flex-direction: column;
  transition: right 0.3s ease-in-out;
  z-index: 1100;
}

.njn-sidebar.open {
  right: 0;
}

.njn-sidebar a {
  color: white;
  margin: 1rem 0;
  text-decoration: none;
  font-size: 1.1rem;
}

.njn-close-btn {
  align-self: flex-end;
  background: none;
  border: none;
  font-size: 2rem;
  color: white;
  margin-bottom: 2rem;
  cursor: pointer;
}

/* Overlay */
.njn-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1050;
}

/* Responsive */
@media (max-width: 1024px) {
  .desktop {
    display: none;
  }

  .njn-hamburger {
    display: block;
  }
}
</style>
