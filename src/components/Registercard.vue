<template>
  <div>
    <v-card
      class="mx-auto pa-12 pb-8 my-10"
      elevation="8"
      max-width="448"
      height="650"
      rounded="lg"
    >
        <h1 class="title-login my-2">Register</h1>

        <v-text-field
            density="compact"
            placeholder="username"
            prepend-inner-icon="mdi-account"
            variant="outlined"
        ></v-text-field>

      <v-text-field
        :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
        :type="visible ? 'text' : 'password'"
        density="compact"
        placeholder="password"
        prepend-inner-icon="mdi-key"
        variant="outlined"
        @click:append-inner="visible = !visible"
      ></v-text-field>

        <v-text-field
            density="compact"
            placeholder="first name"
            prepend-inner-icon="mdi-chevron-right"
            variant="outlined"
        ></v-text-field>

        <v-text-field
            density="compact"
            placeholder="last name"
            prepend-inner-icon="mdi-chevron-right"
            variant="outlined"
        ></v-text-field>

        <v-text-field
            density="compact"
            placeholder="email"
            prepend-inner-icon="mdi-email-outline"
            variant="outlined"
        ></v-text-field>

      <v-btn
        class="mb-8 my-10"
        color="blue"
        size="large"
        variant="tonal"
        block
      >
        Sing Up
      </v-btn>

      <v-btn size="large" variant="tonal"  block flat to="/login">Log in</v-btn>
    </v-card>
  </div>
</template>

<script>
export default {

}
</script>

<style>
    .title-login{
        text-align: center;
    }

    .go-to-sing-up {
        display: flex;
        justify-content: center;
    }
</style>