<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :width="width"
    :height="height"
    :color="color"
    viewBox="0 0 24 24"
    fill="none"
    class="injected-svg"
    data-src="https://cdn.hugeicons.com/icons/diamond-solid-sharp.svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    role="img"
  >
    <path
      d="M11.4697 1.46967C11.7626 1.17678 12.2374 1.17678 12.5303 1.46967L22.5303 11.4697C22.8232 11.7626 22.8232 12.2374 22.5303 12.5303L12.5303 22.5303C12.2374 22.8232 11.7626 22.8232 11.4697 22.5303L1.46967 12.5303C1.17678 12.2374 1.17678 11.7626 1.46967 11.4697L11.4697 1.46967Z"
      :fill="color"
    ></path>
  </svg>
</template>

<script setup lang="ts">
interface DiamondProps {
  width?: string;
  height?: string;
  color?: string;
}

withDefaults(defineProps<DiamondProps>(), {
  width: "12px",
  height: "12px",
  string: "#000000",
});
</script>
