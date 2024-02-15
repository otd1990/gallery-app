<template>
  <svg
    v-if="!disabled"
    width="26"
    height="24"
    viewBox="0 0 26 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
  >
    <path
      d="M1.52789 1.84344L22.0664 12.1127L1.52789 22.382V1.84344Z"
      stroke="black"
      stroke-width="2"
    />
    <rect
      x="24.5791"
      y="0.371478"
      width="0.742956"
      height="23.0316"
      fill="#D8D8D8"
      stroke="black"
      stroke-width="0.742956"
    />
  </svg>

  <svg
    v-else
    width="26"
    height="24"
    viewBox="0 0 26 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
  >
    <g opacity="0.15">
      <path
        d="M1.52734 1.84344L22.0659 12.1127L1.52734 22.382V1.84344Z"
        stroke="black"
        stroke-width="2"
      />
      <rect
        x="24.5785"
        y="0.371478"
        width="0.742956"
        height="23.0316"
        fill="#D8D8D8"
        stroke="black"
        stroke-width="0.742956"
      />
    </g>
  </svg>
</template>

<script setup>
defineProps({
  disabled: {
    type: Boolean,
    default: false
  }
})
</script>
