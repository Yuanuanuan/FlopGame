<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :width="width"
    :height="height"
    :color="color"
    viewBox="0 0 24 24"
    fill="none"
    class="injected-svg"
    data-src="https://cdn.hugeicons.com/icons/favourite-solid-standard.svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    role="img"
  >
    <path
      fill-rule="evenodd"
      clip-rule="evenodd"
      d="M1 8.99835C1 5.358 3.3947 1.99998 7.43 1.99998C9.30819 1.99998 10.7026 2.80469 11.75 4.01044C12.7974 2.80469 14.1918 1.99998 16.07 1.99998C20.1053 1.99998 22.5 5.358 22.5 8.99835C22.5 12.16 21.0352 14.697 19.0211 16.7123C17.0165 18.718 14.4321 20.2456 12.0855 21.4194C11.8743 21.525 11.6257 21.525 11.4145 21.4194C9.06793 20.2456 6.48346 18.718 4.47892 16.7123C2.46481 14.697 1 12.16 1 8.99835Z"
      :fill="color"
    ></path>
  </svg>
</template>

<script setup lang="ts">
interface HeadrtProps {
  width?: string;
  height?: string;
  color?: string;
}

withDefaults(defineProps<HeadrtProps>(), {
  width: "12px",
  height: "12px",
  color: "#000000",
});
</script>
