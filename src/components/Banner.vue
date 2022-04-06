<template>
  <div id="main-banner">
      <p>Make Your Fit Shake</p>
  </div>
</template>

<script>
export default {
    name: "Banner"
}
</script>

<style scoped>
    #main-banner {
        background-image: url('/img/shake.jpg');
        background-size: cover;
        height: 500px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    #main-banner {
        color: #FFF;
        text-align: center;
        font-size: 3.75em;
        background-color: #A13333;
        padding: 20px 40px;
    }
    p {
        background-color: #A13333;
        border-radius: 10px;
    }
</style>