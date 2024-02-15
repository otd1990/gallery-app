<template>
    <article class="slideshow">
        <section class="slideshow__wrapper">
            <TheSlideshow />
        </section>
    </article>
</template>

<script setup>
import TheSlideshow from "../components/TheSlideshow.vue";
</script>

<style>
@import url("../assets/base.css");

.slideshow {
    height: 100%;
    min-height: 100%;
}
</style>