<template>
  <div class="d-flex justify-center">© {{ year }} Aleksandar Trpkovski</div>
</template>

<script>
export default {
  computed: {
    year() {
      const date = new Date();
      return date.getFullYear();
    },
  },
};
</script>

<style scoped></style>
