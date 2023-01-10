<template>
  <div class="flex items-center justify-center bg-gray-700 w-full shadow border boder-gray-500 h-52 cursor-pointer hover:shadow-neutral-900">
    <slot/>
  </div>
</template>

<script>
import Item from "./Item.vue";

export default {
  name: "Box",
  components: {Item}
}
</script>

<style scoped>

</style>