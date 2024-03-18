<template>
  <article class="gallery">
    <section class="home__wrapper masonry-grid">
      <div
        v-for="(group, groupIndex) in columnGroups"
        :key="groupIndex"
        :class="`column-group-${groupIndex} column-grid`"
      >
        <router-link
          v-for="itemIndex in group"
          :key="itemIndex"
          :to="`/slide-show/${itemIndex}`"
          class="masonry-item"
        >
          <article class="gallery-card flex items-center justify-center">
            <img :src="data[itemIndex].images.thumbnail" alt="Painting" class="gallery-card-img" />
            <aside
              :id="`painingName--${data[itemIndex].name.replace(/\s/g, '')}`"
              class="home__painting-info"
            >
              <h2 class="home__painting-name">
                {{ data[itemIndex].name }}
              </h2>
              <span class="home__artist subheading-two">{{ data[itemIndex].artist.name }}</span>
            </aside>
          </article>
        </router-link>
      </div>
    </section>
  </article>
</template>

<script setup>
import data from '../data.json'

const columnGroups = [
  [0, 4, 8, 11],
  [1, 5, 9, 12],
  [2, 6, 13],
  [3, 7, 10, 14]
]
</script>

<style>
@import url('../assets/base.css');

.gallery {
  padding: 0 3rem 1rem 3rem;
}

.gallery__item-wrapper {
  background-repeat: no-repeat;
  background-size: cover;
}

.gallery-card {
  position: relative;
  width: 100%;
  height: 100%;
  display: grid;
  grid-template-rows: 1fr auto;
}

.gallery-card img {
  grid-row: 1 / -1;
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.home__painting-info {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1.8rem;
  background: linear-gradient(180deg, transparent 50%, rgba(0, 0, 0, 1) 100%);
  box-sizing: border-box;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-end;
  transition: all 0.3s;
}

.masonry-item:hover .home__painting-info {
  background: rgba(255, 255, 255, 0.6);
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.5) 25%, transparent 100%);
}

.masonry-grid {
  max-width: 1440px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 1rem;
  margin-bottom: 1rem;
}

.masonry-item {
  break-inside: avoid;
}

.home__artist.subheading-two,
.home__painting-name {
  color: var(--colour-white);
}

.column-grid {
  display: grid;
  grid-template-rows: 1fr;
  grid-row-gap: 16px;
}

@media (max-width: 769px) {
  .masonry-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 545px) {
  .masonry-grid {
    grid-template-columns: 1fr;
  }
}
</style>
