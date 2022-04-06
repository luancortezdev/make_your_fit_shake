<template>
  <div>
    <Banner />
    <div class="main-container">
      <h1>Monte o seu shake</h1>
      <ShakeForms />
    </div>
  </div>
</template>

<script>
import Banner from '../components/Banner.vue';
import ShakeForms from '../components/ShakeForms.vue';
export default {
  components: { Banner, ShakeForms },
  name: "Home",
};
</script>
