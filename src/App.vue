<script setup>
import NavBar from './components/NavBar.vue'
import Hero from './components/Hero.vue'
import FavouriteCollection from './components/FavouriteCollection.vue'
import Contact from './components/Contact.vue'
import Footer from './components/Footer.vue'
import RTLToggle from './components/UI/RTLToggle.vue'
</script>

<template>
  <NavBar />
  <Hero />
  <FavouriteCollection />
  <Contact />
  <Footer />
  <RTLToggle :showInfo="true" :persistState="true" storageKey="rtl-mode" />
</template>

<style scoped></style>
