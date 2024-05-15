<template>
  <div>
    <v-card
        :loading="loading"
        width="100%"
        height="500"
        color="#FAFAFA"
        :style="backgroundStyle"
    >
    <h3 class="header-text">"WRISTWATCH"</h3>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: false,
      backgroundStyle: {
        backgroundImage: `url(${require('../assets/homebackground.png')})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center'
      }
    };
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Prompt:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Ubuntu:ital,wght@0,300;0,400;0,500;0,700;1,300;1,400;1,500;1,700&display=swap');

.header-text {
  color: #acacac;
  text-align: center;
  padding-top: 8%;
  font-size: 118px;
  font-family: "Ubuntu", sans-serif;
}
</style>
