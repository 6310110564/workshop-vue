<template>
  <div>
      <Homeheader />
      <!-- <IntrowatchVue /> -->
      <!-- <InspairVue /> -->
      <AllProductVue />
      <FooterVue /> 
  </div>
</template>

<script>
import FooterVue from '@/components/Footer.vue'
import Homeheader from '../components/Homeheader.vue'
// import ProductVue from '@/components/Product.vue'
// import InspairVue from '@/components/Inspair.vue'
import AllProductVue from '@/components/AllProduct.vue'
// import IntrowatchVue from '@/components/Introwatch.vue'

export default {
  name: 'Home',

  components: {
    Homeheader,
    FooterVue,
    // ProductVue,
    // InspairVue,
    AllProductVue,
    // IntrowatchVue
  },
}
</script>
