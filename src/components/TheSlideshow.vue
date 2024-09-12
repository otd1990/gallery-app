<template>
  <article class="slideshow__main">
    <section class="slideshow__left">
      <article class="slideshow__image">
        <div class="slideshow__image-container">
          <img :src="slideToShow.images.hero.small" :alt="slideToShow.name" />
          <button
            @click="viewImage(slideToShow.images.hero.small, slideToShow.name)"
            class="view-image__button"
          >
            <ViewButtonIcon />
            <span>VIEW IMAGE</span>
          </button>
          <aside class="slideshow-artist__image">
            <img :src="slideToShow.artist.image" :alt="slideToShow.artist.name" />
          </aside>
        </div>
        <aside class="slideshow__title-container">
          <h1 class="slideshow__title">{{ slideToShow.name }}</h1>
          <p class="slideshow__artist susbheading-one">{{ slideToShow.artist.name }}</p>
        </aside>
      </article>
    </section>
    <section class="slideshow__right">
      <article class="slideshow__desc-container">
        <article class="slideshow__desc">
          <section class="slideshow__desc--text-date">
            <aside class="slideshow__date">{{ slideToShow.year }}</aside>
            <p class="slideshow__text">{{ slideToShow.description }}</p>
          </section>
        </article>
      </article>
    </section>
  </article>
  <article class="slideshow__footer">
    <span class="progress__bar" :style="{ width: progress + 'px' }"></span>
    <aside class="slideshow__footer-artist">
      <h3 class="slideshow__footer-title">{{ slideToShow.name }}</h3>
      <p class="slideshow__footer-artist-name susbheading-two">{{ slideToShow.artist.name }}</p>
    </aside>
    <section class="icons">
      <router-link
        :to="`/slide-show/${currentSlide - 1}`"
        :disabled="prevDisabled"
        :style="{ pointerEvents: prevDisabled ? 'none' : 'auto' }"
      >
        <PrevIcon class="icons__prev" :disabled="prevDisabled" />
      </router-link>
      <router-link
        :to="`/slide-show/${currentSlide === data.length - 1 ? currentSlide : currentSlide + 1}`"
        :disabled="nextDisabled"
        :style="{ pointerEvents: nextDisabled ? 'none' : 'auto' }"
      >
        <NextIcon class="icons__next" :disabled="nextDisabled" />
      </router-link>
    </section>
  </article>
  <TheModal :show-modal="showModal" @closeModal="handleCloseModal">
    <template #content>
      <article class="slideshow-modal__content">
        <img :src="modalImage" :alt="modalImageAlt" class="slideshow-modal__image" />
      </article>
    </template>
  </TheModal>
</template>

<script setup>
import { ref, watch, onMounted, onUnmounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import ViewButtonIcon from './icons/ViewButtonIcon.vue'
import NextIcon from './icons/NextIcon.vue'
import PrevIcon from './icons/PrevIcon.vue'
import TheModal from './TheModal.vue'
import data from '../data.json'

defineEmits(['showModal'])

const route = useRoute()
const router = useRouter()

const currentSlide = ref(parseInt(route.params.slide))
const slideToShow = ref(data[currentSlide.value])
const modalImage = ref('')
const modalImageAlt = ref('')
const showModal = ref(false)
const prevDisabled = ref(true)
const nextDisabled = ref(false)
const progress = ref(0)

let slideShowTimeout = undefined

const viewImage = (image, imageName) => {
  modalImage.value = image
  modalImageAlt.value = imageName
  showModal.value = true
}

const handleCloseModal = () => {
  showModal.value = false
}

const checkDisabled = (slide) => {
  const dataLength = data.length - 1
  if (slide > 0) {
    prevDisabled.value = false
  } else {
    prevDisabled.value = true
  }

  if (slide == dataLength) {
    nextDisabled.value = true
  } else {
    nextDisabled.value = false
  }
}

const progressBar = () => {
  const footerWidth = document.querySelector('.slideshow__footer')
  const totalSlides = data.length - 1
  let prog = Math.floor((footerWidth.offsetWidth / totalSlides) * currentSlide.value)

  progress.value = prog
}

const slideShowAuto = () => {
  if (slideShowTimeout) {
    clearTimeout(slideShowTimeout)
  }

  slideShowTimeout = setTimeout(() => {
    goToNext(currentSlide.value + 1)
  }, 5000)
}

const goToNext = (newSlide) => {
  if (currentSlide.value === data.length - 1) return
  currentSlide.value = newSlide
  slideToShow.value = data[currentSlide.value]

  router.push({
    name: 'slideShow',
    params: { slide: newSlide },
    query: { autoplay: 'true' }
  })
}

watch(
  () => route.params,
  (newSlide) => {
    if (route.query.autoplay === 'true') {
      slideShowAuto()
      return
    }
    goToNext(parseInt(newSlide.slide))
    checkDisabled(parseInt(newSlide.slide))
    progressBar()
  }
)

onMounted(() => {
  checkDisabled(parseInt(route.params.slide))
  progressBar()

  if (route.query.autoplay === 'true') {
    slideShowAuto()
  }
})

onUnmounted(() => {
  console.log('SLide show unmounted, clear the autoplay')
  clearTimeout(slideShowTimeout)
})
</script>

<style>
.slideshow__main {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-start;
  justify-content: space-between;
  gap: 1.25rem;
  flex: 1 0 100%;
  margin-bottom: 2rem;

  max-width: 1440px;
  width: 100%;
  padding: 0 3rem;
  margin: 0 auto;
}

.slideshow__wrapper {
  min-height: 100%;
  display: flex;
  flex-direction: column;
}

.slideshow__image-container {
  position: relative;
}

.slideshow__image {
  display: flex;
  flex-direction: row;
  align-items: flex-end;
  position: relative;
  height: 560px;
  width: 475px;
  object-fit: fill;
}

.slideshow__image img {
  width: 100%;
  height: 100%;
}

.slideshow__title-container {
  background: #fff;
  padding: 0.5rem 2rem;
  min-height: 15rem;
  position: absolute;
  top: 0;
  left: 65%;
}

.slideshow__artist {
  color: var(--colour-gray-dark);
}

.slideshow__date {
  font-size: 20rem;
  line-height: 15rem;
  color: var(--colour-gray-light);
}

.slideshow__desc {
  display: flex;
  justify-content: space-between;
  align-items: end;
  gap: 2rem;

  max-width: 40rem;
  font-size: 1.25rem;
  margin: 0 auto;

  color: var(--colour-gray-dark);

  z-index: 1;
  position: relative;
}

.slideshow__desc--text-date {
  text-align: center;
}

.slideshow__footer {
  padding: 1rem 1.5rem 1.5rem 1.5rem;
  border-top: 1px solid var(--colour-gray-mid);

  display: flex;
  align-items: center;
  justify-content: space-between;

  margin-top: 2rem;

  position: relative;
}

.progress__bar {
  background-color: var(--colour-black);
  height: 1px;
  position: absolute;
  top: -1px;
  left: 0;
  transition: all 0.3s;
}

.icons__next {
  margin-left: 1.25rem;
}

.icons button {
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.view-image__button {
  min-width: 150px;
  padding: 0.75rem 1.5rem;
  text-transform: uppercase;
  position: absolute;
  bottom: 1rem;
  left: 1rem;
  letter-spacing: 1.5px;
  background-color: var(--colour-black);
  border: none;
  box-shadow: none;
  color: var(--colour-gray-light);
  font-family: var(--font-main);
  font-size: 12px;
  cursor: pointer;
}

.view-image__button span {
  display: inline-block;
  margin-left: 8px;
}

.slideshow-artist__image {
  position: absolute;
  bottom: 1rem;
  right: 1rem;
}

.slideshow-artist__image img {
  display: block;
  height: 5rem;
  width: 5rem;
}

@media (max-width: 1440px) {
  .slideshow__desc {
    margin-left: 2.25rem;
  }
}

@media (max-width: 1365px) {
  .slideshow__main {
    align-items: center;
  }

  .slideshow__date {
    font-size: 12rem;
    line-height: 10rem;
  }
}
@media (max-width: 1280px) {
  .slideshow__main {
    justify-content: center;
  }

  .slideshow__desc {
    max-width: 100%;
    margin: 1rem 0;
  }

  .slideshow__image {
    height: auto;
    width: auto;
  }

  .slideshow__title-container {
    min-height: auto;
    position: initial;
    top: unset;
    left: unset;
  }
}

@media (max-width: 900px) {
  .slideshow__date {
    font-size: 7rem;
    line-height: 5rem;
  }

  .slideshow__text {
    font-size: 1.15rem;
  }

  .slideshow__title {
    font-size: 2rem;
    line-height: 2.5rem;
    margin-bottom: 1rem;
  }
}

@media (max-width: 650px) {
  .slideshow__image {
    flex-wrap: wrap;
  }

  .slideshow__image-container {
    order: 2;
  }

  .slideshow__title-container {
    order: 1;
    text-align: center;
    margin: 0 auto;
  }
}
</style>
