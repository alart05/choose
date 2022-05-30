<template>
  <div>
    <Header />
    <Slider/>
  </div>
</template>

<script>
import Header from '@/components/Header.vue'
import Slider from '@/components/Slider.vue'

export default {
  name: 'App',
  components: {
    Header,
    Slider
  }
}
</script>

<style lang="scss">
  @font-face {
    font-family: "Inter";
    font-style: normal;
    font-weight: 400;
    src: url("assets/fonts/Inter/Inter-Regular.ttf");
  }
  @font-face {
    font-family: "Inter";
    font-style: normal;
    font-weight: 700;
    src: url("assets/fonts/Inter/Inter-Bold.ttf");
  }
  @font-face {
    font-family: "Nexa";
    font-style: normal;
    font-weight: 700;
    src: url("assets/fonts/Nexa/Nexa-Bold.otf");
  }
  body {
    font-family: Inter;
    margin: 0;
  }
</style>
