<template>
  <v-carousel
    cycle
    height="400"
    hide-delimiter-background
    show-arrows-on-hover
  >
    <v-carousel-item
      v-for="(slide, i) in slides"
      :key="i"
    >
      <v-sheet
        color="#1E1C24"
        height="100%"
      >
        <v-row
          class="fill-height"
          align="center"
          justify="center"
        >
          <div class="text-h2">
            <v-row>
                <v-cal class="card-product">
                    <v-card width="450" height="300" class="mx-auto">
                        <v-card-title>Product1</v-card-title>
                        <v-card-text>product1</v-card-text>
                    </v-card>
                </v-cal>
                 <v-cal class="card-product">
                    <v-card width="450" height="300">
                        <v-card-title>Product2</v-card-title>
                        <v-card-text>product2</v-card-text>
                    </v-card>
                </v-cal>
                <v-cal class="card-product">
                    <v-card width="450" height="300">
                        <v-card-title>Product3</v-card-title>
                        <v-card-text>product3</v-card-text>
                    </v-card>
                </v-cal>
            </v-row>
          </div>
        </v-row>
      </v-sheet>
    </v-carousel-item>
  </v-carousel>
</template>

<script>
  export default {
    data () {
      return {
        colors: [
          'indigo',
          'warning',
          'pink darken-2',
          'red lighten-1',
          'deep-purple accent-4',
        ],
        slides: [
          'First',
          'Second',
          'Third',
          'Fourth',
          'Fifth',
        ],
      }
    },
  }
</script>
<style>
.card-product{
    margin: 0 20px;
}
</style>