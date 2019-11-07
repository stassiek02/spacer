<template>
  <div class="heroImage" />
</template>

<script>
export default {
  name: 'HeroImage',
};
</script>

<style lang="scss" scoped>
.heroImage {
  position: fixed;
  top: 0;
  z-index: -1;
  width: 100%;
  min-height: 100vh;
  height: 100vh;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 80% 0%;
  background-image: url('../assets/heroimage.jpg');
}
.heroImage::after {
  content: '';
  background: black;
  opacity: 0.25;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
  width: 100%;
  height: 100vh;
}
</style>