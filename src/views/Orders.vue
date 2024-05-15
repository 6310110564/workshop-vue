<template>
  <div>
    <v-card
      class="mx-auto my-6"
      width="80%"
      v-for="(order, orderIndex) in orders" :key="orderIndex"
    >

    <h3 class="text-title-order">หมายเลขคำสั่งซื้อ : {{ order.order_id }}</h3>
      <v-container>
        <v-row>
            <v-col cols="10">
                <v-row v-for="(item, itemIndex) in order.cart" :key="itemIndex">
                    <v-col cols="12" sm="4">
                        <img :src="item.product.product_img" alt="Product Image" height="150">
                    </v-col>
                    <v-col cols="12" sm="4">
                        <v-sheet class="ma-2 pa-2">
                            <v-card-title><h2>{{ item.product.product_name }}</h2></v-card-title>
                            <v-card-text>Price: {{ item.product.price }}</v-card-text>
                            <v-card-text>Amount: {{ item.amount }}</v-card-text>
                        </v-sheet>
                        <v-divider></v-divider>
                    </v-col>
                </v-row>
                <v-divider></v-divider>
            </v-col>
            <v-col>
                <v-btn color="error" class="my-16 pa-6">Checkout</v-btn>
            </v-col>
        </v-row>
      </v-container>
    </v-card>
  </div>
</template>

<script>
export default {
    data() {
        return {
            orders: [],
        }
    },

    created() {
        this.getData()
    },

    methods: {
        getData() {
            this.axios.get('http://localhost:3000/orders')
                .then((response) => {
                console.log('response: ', response.data.data)
                this.orders = response.data.data
            })
        },
    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Prompt:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

    .text-title-order{
        text-align: center;
        margin-top: 20px;
        padding: 40px 0;
        font-family: "Prompt", sans-serif;
        font-weight: 400;
        font-style: normal;
    }
</style>