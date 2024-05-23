<template>
  <a :href="link.href" target="_blank" class="flex">
    <div class="flex items-center gap-4 justify-center">
      <UIcon :name="link.icon" dynamic />

      <p>{{ link.label }}</p>
    </div>
  </a>
</template>
<script setup>
const { link } = defineProps({
  link: { default: { href: "", icon: "", label: "" } },
});
</script>
