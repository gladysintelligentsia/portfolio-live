<script setup>
import Navbar from './components/Navbar.vue';
import Banner from './components/Banner.vue';
import Projects from './components/Projects.vue';
import Contact from './components/Contact.vue';
import Footer from './components/Footer.vue';
</script>

<template>
  <div>
    <Navbar />
    <Banner />
    <Projects />
    <Contact />
    <Footer />
  </div>
</template>

<style>
/* Global resets or layout styles can go here */
body {
  margin: 0;
  font-family: sans-serif;
}
</style>