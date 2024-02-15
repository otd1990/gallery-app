<template>
  <article class="header">
    <section class="header__logo">
      <RouterLink to="/"><GallerriaHeading /></RouterLink>
    </section>
    <section class="header__start">
      <router-link to="/slide-show/0?autoplay=true" class="header__start-button"
        >Start Slideshow</router-link
      >
    </section>
  </article>
</template>

<script setup>
import GallerriaHeading from './icons/GallerriaHeading.vue'
</script>

<style>
@import url('../assets/base.css');

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
  border-bottom: 1px solid var(--colour-gray-mid);
}

.header__start-button {
  font-family: var(--font-main);
  color: var(--colour-gray-dark);
  background-color: transparent;
  border: none;
  text-transform: uppercase;
  letter-spacing: 3px;
  cursor: pointer;
}
</style>
