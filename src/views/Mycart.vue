<template>
  <div>
        <v-card
            class="mx-auto my-3"
            width="95%"
            height="200"

            v-for="(cart, index) in carts" :key = "index"
        >
            <!-- <v-card-title>{{cart.product.product_name}}</v-card-title>
            <v-img :src="cart.product.product_img" height="40%"></v-img>
            <v-card-text>{{ cart.product.price }}</v-card-text>
            <v-divider width="95%" class="mx-auto my-1"></v-divider>

            <v-card-actions class="action-order">
            <v-btn @click="decrement(index)"> - </v-btn>
                <span class="text-amout"><p>{{ cart.amount }}</p></span>
            <v-btn @click="increment(index)"> + </v-btn>

            <v-btn color="error" @click="saveDeleteCart(cart._id)">Delete</v-btn>
                <v-btn color="primary">Buy</v-btn>
            </v-card-actions>
            <v-divider></v-divider> -->

            <v-container>
                <v-row >
                <v-col cols="12" sm="4" >
                    <v-sheet>
                        <v-img :src="cart.product.product_img" class="img-products"></v-img>
                    </v-sheet>
                </v-col>
                <v-col cols="12" sm="4">
                    <v-sheet class="ma-2 pa-2">
                        <v-card-title>{{cart.product.product_name}}</v-card-title>
                        <v-card-text>{{ cart.product.price }}</v-card-text>
                        <v-card-text>ราคารวม : {{sumPrice(cart.product.price, cart.amount)}}</v-card-text>
                        
                    </v-sheet>
                </v-col>
                <v-col cols="12" sm="4">
                    <v-sheet class="ma-2 pa-2">
                        <v-card-actions class="action-order">
                        <v-btn @click="decrement(index, cart)"> - </v-btn>
                            <span class="text-amout"><p>{{ cart.amount }}</p></span>
                        <v-btn @click="increment(index, cart)"> + </v-btn>

                        <v-btn color="error" @click="saveDeleteCart(cart.cart_id)">Delete</v-btn>
                        </v-card-actions>
                    </v-sheet>
                </v-col>
                </v-row>
            </v-container>
        </v-card>

        <v-card
            color="#3c3434"
            class="mx-auto my-6"
            width="95%"
            height="90"
        >
            <v-container class="bg-surface-variant">
                <v-row no-gutters>
                    <v-col cols="6" sm="6">
                            <h3 class="text-sum">ราคารวมทั้งหมด</h3>
                    </v-col>
                    <v-col cols="12" sm="2">
                        <h3 class="text-sum">{{ totalSum }}</h3>
                    </v-col>
                    <v-col cols="12" sm="2">
                        <h3 class="text-sum">บาท</h3>
                    </v-col>
                    <v-col cols="12" sm="2">
                        <v-btn color="primary" class="my-4 text-sum" @click="postOrders">สั่งซื้อสินค้า</v-btn>
                    </v-col>
                </v-row>
                
                
            </v-container>
        </v-card>

  </div>
</template>

<script>
import Swal from 'sweetalert2'

export default {
      data() {
        return {
            carts: [],
            products: [],
            putdata: { amount: '' },
        }
    },

    created() {
        this.getData()
        this.getProductsData()
        this.carts.forEach(cart => {
            this.getProductsDataID(cart.product_id);
        });
    },

    mounted() {
        this.sumAmount();
    },

    computed: {
        totalSum() {
            let sum = 0;
            if (this.carts) {
                this.carts.forEach(item => {
                    sum += this.sumPrice(item.product.price, item.amount);
                });
            }
            return sum;
        }
    },


    methods: {
        getData() {
            this.axios.get('http://localhost:3000/carts')
                .then((response) => {
                console.log('response: ', response)
                this.carts = response.data.data
            })
        },

        getProductsData() {
            this.axios.get('http://localhost:3000/products')
                .then((response) => {
                console.log('response: ', response)
                this.products = response.data.data
            })
        },

        async saveDeleteCart (item) {
            try {
                // const result = await Swal.fire({
                //     title: 'Delete?',
                //     icon: 'warning',
                //     showCancelButton: true,
                //     confirmButtonText: 'Yes',
                //     cancelButtonText: 'No'
                // });

                // if (result.isConfirmed) {
                    const { data } = await this.axios.delete('http://localhost:3000/carts/' + item);
                    console.log(data);
                    // await Swal.fire({
                    //     icon: 'success',
                    //     title: 'Delete success',
                    // });
                    this.getData();
                    // this.closeData()
                // }
            } catch (error) {
                console.log(error);
                Swal.fire({
                    icon: 'error',
                    title: 'Error',
                });
                alert('item: ', item);
            }
        },

        async putAmountCart(item) {
            console.log('item count: ', item)
            this.putdata.amount = item.amount;

            const url = `http://localhost:3000/carts/${item.cart_id}`;

            try {
                const response = await this.axios.put(url, this.putdata);
                console.log(response.data);
            } catch (error) {
                console.error(error);
                Swal.fire({
                    icon: 'error',
                    title: 'Error',
                });
            }
        },

        increment(index, cart) {
            if (this.carts[index].amount < this.products[index].stock) {
                this.carts[index].amount += 1;
                console.log('cart: ', cart)
                this.putAmountCart(cart)
            }
            console.log('index: ', index, ' amount: ', this.carts[index].amount)
        },

        decrement(index, cart) {
            if (this.carts[index].amount > 1) {
                this.carts[index].amount -= 1;
                console.log('cart: ', cart)
                this.putAmountCart(cart)
            }
            console.log('index: ', index, ' amount: ', this.carts[index].amount)
        },

        sumAmount() {
            const totalAmount = this.carts.reduce((total, cart) => total + cart.amount, 0);
            return totalAmount
        },

        sumPrice(price, amount) {
            return price * amount
        },

        //test
        async postOrders() {
            try {
                const cartIds = [];

                for (const cartItem of this.carts) {
                    cartIds.push(cartItem.cart_id);
                }
                const orderData = {
                    customer_id: '663c95e389ee92e51e6db26d',
                    cart_id: cartIds
                };
                const response = await this.axios.post('http://localhost:3000/orders', orderData);
                console.log(response);

                await Swal.fire({
                        icon: 'success',
                        title: 'สั่งซื้อสินค้าเรียบร้อย',
                });

                //ลบสินค้าจากตระกร้า
                // for (const cartItem of this.carts) {
                //     await this.saveDeleteCart(cartItem.cart_id);
                // }
                // this.carts = [];

            } catch (error) {
                console.error('Error posting orders:', error);
            }
        }

    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Prompt:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

    .action-order{
        display: flex;
        justify-content: end;
        margin: 0 20px;
    }

    .img-products {
        height: 160px;
        width: auto;
        margin: auto;
    }

    .text-sum {
        padding-top: 20px;
        color: aliceblue;
        font-family: "Prompt", sans-serif;
        font-weight: 400;
        font-style: normal;
    }
</style>