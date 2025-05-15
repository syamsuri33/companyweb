<script setup>
import { ref, onMounted } from 'vue';

const navigation = [
  { name: 'Home', href: '#home' },
  { name: 'About', href: '#about' },
  { name: 'Portfolio', href: '#portfolio' },
  { name: 'Team', href: '#team' },
  { name: 'Contact', href: '#contact' },
];

const isMenuOpen = ref(false);

function toggleMenu() {
  isMenuOpen.value = !isMenuOpen.value;
  if (isMenuOpen.value) {
    document.body.classList.add('menu-open');
  } else {
    document.body.classList.remove('menu-open');
  }
}

function closeMenu() {
  isMenuOpen.value = false;
  document.body.classList.remove('menu-open');
}

function scrollToSection(href) {
  const section = document.querySelector(href);
  if (section) {
    section.scrollIntoView({ behavior: 'smooth' });
    closeMenu();
  }
}

const sectionRefs = ref([]);

onMounted(() => {
  const observerOptions = {
    root: null,
    rootMargin: '0px',
    threshold: 0.1,
  };

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('animate');
      } else {
        entry.target.classList.remove('animate');
      }
    });
  }, observerOptions);

  sectionRefs.value = navigation.map(item => document.querySelector(item.href));
  sectionRefs.value.forEach(section => {
    if (section) observer.observe(section);
  });
});
</script>

<template>
  <div class="w-screen min-h-screen bg-blue-100">
    <nav class="bg-white shadow-md fixed top-0 left-0 right-0 z-50">
      <div class="mx-auto px-4 sm:px-6 lg:px-8 flex items-center justify-between h-16">
        <div class="text-2xl font-bold text-blue-600 select-none">MySite</div>
        <!-- Desktop Menu -->
        <ul class="lg:gap-x-6 hidden md:flex space-x-8 text-gray-700 font-medium">
          <li v-for="item in navigation" :key="item.name">
            <a :href="item.href" @click.prevent="scrollToSection(item.href)" class="hover:text-blue-600 transition">{{
              item.name }}</a>
          </li>
        </ul>

        <!-- Hamburger Icon -->
        <button @click="toggleMenu" :aria-expanded="isMenuOpen.toString()" aria-label="Menu"
          class="md:hidden hamburger focus:outline-none">
          <span class="md:hidden hamburger-line" :class="{ 'rotate-45 top-1.5': isMenuOpen, 'top-0': !isMenuOpen }"
            style="transition: all 0.3s ease-in-out;"></span>
          <span class="md:hidden hamburger-line" :class="{ 'opacity-0': isMenuOpen, 'top-6': !isMenuOpen }"
            style="transition: all 0.3s ease-in-out;"></span>
          <span class="md:hidden hamburger-line" :class="{ '-rotate-45 top-6': isMenuOpen, 'top-12': !isMenuOpen }"
            style="transition: all 0.3s ease-in-out;"></span>
        </button>
      </div>

      <!-- Mobile Menu -->
      <div
        :class="['menu fixed top-16 left-0 w-3/4 max-w-xs h-full bg-white shadow-lg md:hidden flex flex-col space-y-6 p-6 transform transition-transform duration-300', { 'translate-x-0': isMenuOpen, '-translate-x-full': !isMenuOpen }]">
        <a v-for="item in navigation" :key="item.name" :href="item.href" @click.prevent="scrollToSection(item.href)"
          class="text-lg font-semibold text-gray-800 hover:text-blue-600 transition">
          {{ item.name }}
        </a>
      </div>
    </nav>

    <main class="w-screen pt-10 mx-auto">
      <section id="home"
        class="min-h-screen flex items-center justify-center bg-gradient-to-br from-primary to-secondary">
        <div class="container mx-auto px-6 py-20 flex flex-col md:flex-row items-center">
          <div class="md:w-1/2 text-center md:text-left mb-10 md:mb-0">
            <h1 class="text-4xl md:text-6xl font-bold text-white mb-6">Innovative Software Solutions</h1>
            <p class="text-xl text-white opacity-90 mb-8">We craft cutting-edge software that drives business growth and
              digital transformation.</p>
            <div class="flex flex-col sm:flex-row gap-4 justify-center md:justify-start">
              <button class="btn btn-accent btn-lg">Get Started</button>
              <button class="btn btn-outline btn-lg text-white">Learn More</button>
            </div>
          </div>
        </div>
      </section>

      <section id="about"
        class="h-screen flex flex-col items-center justify-center bg-white rounded-lg shadow-md mb-8 opacity-0 transform translate-y-6 transition-all duration-700 ease-out">
        <h1 class="text-5xl font-bold text-blue-600 mb-4">About</h1>
        <p class="text-gray-700 text-center max-w-xl">This About section gives more info about the site and its purpose.
        </p>
      </section>

      <section id="portfolio"
        class="h-screen flex flex-col items-center justify-center bg-white rounded-lg shadow-md mb-8 opacity-0 transform translate-y-6 transition-all duration-700 ease-out">
        <h1 class="text-5xl font-bold text-blue-600 mb-4">Product</h1>
        <p class="text-gray-700 text-center max-w-xl">Reach out to us via this Contact section. We're happy to hear from
          you!</p>
      </section>

      <section id="team"
        class="h-screen flex flex-col items-center justify-center bg-white rounded-lg shadow-md mb-8 opacity-0 transform translate-y-6 transition-all duration-700 ease-out">
        <h1 class="text-5xl font-bold text-blue-600 mb-4">Team</h1>
        <p class="text-gray-700 text-center max-w-xl">Reach out to us via this Contact section. We're happy to hear from
          you!</p>
      </section>

      <section id="contact"
        class="h-screen flex flex-col items-center justify-center bg-white rounded-lg shadow-md mb-8 opacity-0 transform translate-y-6 transition-all duration-700 ease-out">
        <h1 class="text-5xl font-bold text-blue-600 mb-4">Contact</h1>
        <p class="text-gray-700 text-center max-w-xl">Reach out to us via this Contact section. We're happy to hear from
          you!</p>
      </section>
    </main>
  </div>
</template>

<style scoped>
@media (min-width: 1024px) {
  .hamburger {
    /* display: none !important; */
  }
}

.hamburger {
  cursor: pointer;
  width: 24px;
  height: 18px;
  position: relative;
  display: inline-block;
}

.hamburger-line {
  position: absolute;
  left: 0;
  width: 100%;
  height: 3px;
  background: #2563eb;
  /* Tailwind blue-600 */
  border-radius: 4px;
}

.hamburger-line:nth-child(1) {
  top: 0;
  transition: all 0.3s ease-in-out;
}

.hamburger-line:nth-child(2) {
  top: 7.5px;
  transition: all 0.3s ease-in-out;
}

.hamburger-line:nth-child(3) {
  top: 15px;
  transition: all 0.3s ease-in-out;
}

.menu {
  transition: transform 0.3s ease-in-out;
  /* Initially off screen */
  transform: translateX(-100%);
}

.menu.translate-x-0 {
  transform: translateX(0);
}

/* Scroll animation styles */
.animate {
  opacity: 0.8 !important;
  transform: translateY(0) !important;
}
</style>
