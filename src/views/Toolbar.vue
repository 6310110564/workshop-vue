<template>
  <div>
    <v-app-bar
      app
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
        <v-btn flat to="/login"><h2>WHAT IS WRISTWATCH</h2></v-btn>

        <!-- <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        /> -->
        <!-- <h2>WHAT IS WATCH</h2> -->
      </div>

      <v-spacer></v-spacer>

      <!-- <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn> -->

      <v-toolbar-items>
        <v-btn flat to="/">Home</v-btn>
        <v-btn flat to="/manage-product">Product</v-btn>
        <v-btn flat to="/mycart"><v-icon>mdi-cart</v-icon></v-btn>
        <v-btn flat to="/order"><v-icon>mdi-shopping</v-icon></v-btn>
        <!-- <p class="text-carts-amount">{{ sumAmount() }}</p> -->
      </v-toolbar-items>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </div>
</template>

<script>

// import EventBus from '../EvenBus'

export default {

    data() {
        return {
            carts: [],
            
        }
    },

    created() {
        this.getData()

        // EventBus.$on('cartUpdated', () => {
        //   this.getData();
        // });
    },

    // mounted() {
    //     this.sumAmount();
    // },

    methods: {
        getData() {
            this.axios.get('http://localhost:3000/carts')
                .then((response) => {
                this.carts = response.data.data
            })
        },

        sumAmount() {
            const totalAmount = this.carts.reduce((total, cart) => total + cart.amount, 0);
            return totalAmount
        }
    }
}
</script>

<style>
  .text-carts-amount{
    color: #ffffff;
  }
</style>