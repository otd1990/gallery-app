<template>
  <article v-if="showModal" class="modal">
    <div class="modal__inner">
      <section class="modal__header">
        <button @click="$emit('closeModal')" class="modal__button">Close</button>
      </section>
      <section class="modal__content">
        <slot name="content" class="modal__content" />
      </section>
    </div>
  </article>
</template>

<script setup>
defineProps({
  showModal: {
    type: Boolean,
    default: false
  }
})

console.log('the modal')
</script>

<style>
@import url('../assets/base.css');

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.85);
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

.modal__inner {
  padding: 2rem;
}

.modal__button {
  margin-left: auto;
  display: block;
  margin-bottom: 1rem;
  background: transparent;
  color: var(--colour-white);
  text-transform: uppercase;
  letter-spacing: 1px;
}
</style>
