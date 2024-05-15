<template>
  <div>
    <v-item-group>
      <v-container>
        <v-row>
          <v-col cols="3" v-for="(item, index) in products" :key="index">
            <v-item>
              <v-card
                height="450"
                width="300"
              >
                <v-img :src="item.product_img" height="50%"></v-img>
                <h4 class="product_name">{{ item.product_name }}</h4>
                <v-card-text>Price : {{ item.price }}</v-card-text>
                <v-card-text>Stock : {{ item.stock }}</v-card-text>
              
                <v-card-actions>
                  <v-btn @click="decrement(index)"> - </v-btn>
                  <span class="text-amout"><p>{{ item.count }}</p></span>
                  <v-btn @click="increment(index)"> + </v-btn>
                  <!-- <v-btn color="info" @click="addItemToCart(item)">Add</v-btn> -->
                  <v-btn color="info" @click="checkHaveProduct(item)">Add</v-btn>
                </v-card-actions>
              </v-card>
            </v-item>
          </v-col>
        </v-row>
      </v-container>
    </v-item-group>
  </div>
</template>

<script>
import axios from "axios";
import Swal from 'sweetalert2'
// import EventBus from '../EvenBus'

export default {
  data() {
    return {
      products: [],
      carts: [],
      postdata: {
        amount: '',
        user_id: '663c95e389ee92e51e6db26d',
      },

      postdefault: {
        amount: '',
        user_id: '663c95e389ee92e51e6db26d',
      }
    }
  },

  created() {
    this.getDataAllProduct(); 
    this.getDataAllCarts();
  },

  methods: {
    getDataAllProduct() {
      axios
        .get("http://localhost:3000/products")
        .then((res) => {
          this.products = res.data.data.map(item => ({ ...item, count: 1 }));
        })
        .catch((error) => {
          console.log("Error Fetching Data: ", error)
        })
    },

      getDataAllCarts() {
        axios
          .get("http://localhost:3000/carts")
          .then((res) => {
            this.carts = res.data.data;
          })
          .catch((error) => {
            console.log("Error Fetching Data: ", error)
          })
      },

      checkHaveProduct(item) {
          const foundProduct = this.carts.find(cart => cart.product._id === item._id);

          if (foundProduct) {
              this.putAmountCart(foundProduct, item);
          } else {
              this.addItemToCart(item);
          }
      },

    async addItemToCart(item) {
      this.postdata.amount = item.count;

      const url = `http://localhost:3000/products/${item._id}/carts`;

      try {
        const response = await axios.post(url, this.postdata);
        console.log(response.data);
        Swal.fire({
          icon: 'success',
          title: 'Create success!',
        });
        this.getDataAllCarts()
        this.getDataAllProduct()
        // EventBus.$emit('cartUpdated');
        // window.location.reload();
      } catch (error) {
        console.error(error);
        alert('error')
      }
    },

    async putAmountCart(foundProduct, item) {
        console.log('cart_id: ', foundProduct)
        console.log('item count: ', item);
        this.postdata.amount = foundProduct.amount + item.count;

        const url = `http://localhost:3000/carts/${foundProduct.cart_id}`;

        try {
            const response = await axios.put(url, this.postdata);
            console.log(response.data);
            Swal.fire({
              icon: 'success',
              title: 'Update success!',
            });
            this.getDataAllCarts()
            this.getDataAllProduct()
        } catch (error) {
            console.error(error);
            Swal.fire({
                icon: 'error',
                title: 'Error',
            });
        }
    },

    increment(index) {
      if (this.products[index].count < this.products[index].stock) {
        this.products[index].count += 1;
      }
      console.log('index: ', index, ' count: ', this.products[index].count)
    },

    decrement(index) {
      if (this.products[index].count > 1) {
        this.products[index].count -= 1;
      }
      console.log('index: ', index, ' count: ', this.products[index].count)
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Prompt:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Ubuntu:ital,wght@0,300;0,400;0,500;0,700;1,300;1,400;1,500;1,700&display=swap');

.product_name {
  text-align: center;
  background-color: rgb(219, 219, 196);
  padding: 10px 0;
}

.text-amout {
  padding: 20px 20px 0 20px;
}

.swal2-title {
  font-family: "Ubuntu", sans-serif !important;
  font-family: Arial, sans-serif !important;
  font-size: 32px !important;
}

.swal2-content {
  font-family: Arial, sans-serif !important;
  font-size: 32px !important;
}
</style>
