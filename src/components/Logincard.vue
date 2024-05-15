<template>
  <div>
    <v-card
      class="mx-auto pa-12 pb-8 my-16"
      elevation="8"
      max-width="448"
      height="500"
      rounded="lg"
      dark
    >
        <h1 class="title-login my-2">Login</h1>
        <!-- <div class="text-subtitle-1 text-medium-emphasis">Account</div> -->

        <v-text-field
            density="compact"
            placeholder="username"
            prepend-inner-icon="mdi-account"
            variant="outlined"
        ></v-text-field>

        <!-- <div class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between">
            Password

        <a
          class="text-caption text-decoration-none text-blue"
          href="#"
          rel="noopener noreferrer"
          target="_blank"
        >
          Forgot login password?</a>
      </div> -->

      <v-text-field
        :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
        :type="visible ? 'text' : 'password'"
        density="compact"
        placeholder="password"
        prepend-inner-icon="mdi-lock-outline"
        variant="outlined"
        @click:append-inner="visible = !visible"
      ></v-text-field>

      <v-btn
        class="mb-8 my-10"
        color="blue"
        size="large"
        variant="tonal"
        block
      >
        Log In
      </v-btn>

        <span class="go-to-sing-up"><v-btn flat to="/register">Sign up now</v-btn></span>
    </v-card>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
    .title-login{
        text-align: center;
    }

    .go-to-sing-up {
        display: flex;
        justify-content: center;
    }
</style>