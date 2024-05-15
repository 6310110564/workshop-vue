<template>
  <div class="main">
<v-row>
            <v-col cols="12">
                <span class="block-title"><h1 class="manage_product_title">Management Product</h1></span>
                <!-- <h2>{{ apidata }}</h2> -->
                <v-btn color="success" @click="newItem">NewItem</v-btn>
            </v-col>

            <v-col cols="3" v-for="(item, index) in apidata" :key="index"> 
                <v-card height="450" width="300">
                    <v-img :src="item.product_img" height="50%"></v-img>
                    <h4 class="product-name">{{ item.product_name }}</h4>
                    <v-card-text>Price: {{ item.price }}</v-card-text>
                    <v-card-text>Stock: {{ item.stock }}</v-card-text>
                
                    <v-card-actions>
                        <v-btn color="error" @click="saveDelete(item._id)">delete</v-btn>
                        <v-btn color="info" @click="editItem(item)">Edit</v-btn>
                    </v-card-actions>
                </v-card>
            </v-col>
    </v-row>

    <!-- <v-btn color="primary" @click="dialogedit = !dialogedit">Edit</v-btn> -->
    <v-dialog v-model="dialogedit" max-width="500px">
        <v-card>
            <v-card-title primary-title>{{savemode}}</v-card-title>
            <v-card-text>
                <v-row>
                    <v-col cols="12">
                        <v-text-field 
                            name="product_name" 
                            label="product name" 
                            id="product_name" 
                            v-model="postdata.product_name">
                        </v-text-field>
                    </v-col>
                    <v-col cols="12">
                        <v-text-field 
                            name="product_img" 
                            label="product_img" 
                            id="product_img" 
                            v-model="postdata.product_img">
                        </v-text-field>
                    </v-col>
                    <v-col cols="6">
                        <v-text-field 
                            name="price" 
                            label="price" 
                            id="price" 
                            v-model="postdata.price">
                        </v-text-field>
                    </v-col>
                    <v-col cols="6">
                        <v-text-field 
                            name="stock" 
                            label="stock" 
                            id="stock" 
                            v-model="postdata.stock">
                        </v-text-field>
                    
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn text color="error" @click="closeData()">cancel</v-btn>
                            <v-btn text color="info" @click="saveSelect()">save</v-btn>
                        </v-card-actions>
                    </v-col>
                </v-row>
            </v-card-text>
        </v-card>
    </v-dialog>
  </div>
</template>

<script>
import Swal from 'sweetalert2'

export default {
    data () {
        return {
            id: '',
            apidata: [],
            dialogedit: false,
            //เอาไว้ส่งข้อมูล
            postdata: {
                product_name: '',
                product_img: '',
                price: '',
                stock: ''
            },
            //เอาไว้ล้างข้อมูล
            postdefault: {
                product_name: '',
                product_img: '',
                price: '',
                stock: ''
            }
        }
    },

    created () {
        this.getData()
    },

    computed: {
        savemode () {
            return this.id == '' ? 'New Item' : 'Edit Item'
        }
    },

    methods: {
        newItem() {
            this.id = ''
            this.postdata = {...this.postdefault}
            this.dialogedit = true
        },
        editItem(item) {
            this.id = item._id
            this.postdata = item
            this.dialogedit = true
        },
        closeData() {
            this.id = ''
            this.postdata = {...this.postdefault}
            this.dialogedit = false
        },
        saveSelect() {
            if(this.id != ''){
                this.savePutdata()
            }else {
                this.savePostdata()
            }
        },
        getData() {
            this.axios.get('http://localhost:3000/products')
                .then((response) => {
                    console.log('response: ', response)
                    this.apidata = response.data.data
                })
        },
        async savePostdata () {
            try{
                const {data} = await this.axios.post('http://localhost:3000/products', this.postdata)
                console.log(data)
                Swal.fire({
                    icon: 'success',
                    title: 'Create success',
                });
                this.getData()
                this.closeData()
            } catch(error) {
                console.log(error)
                alert('error')
            }
        },
        async savePutdata () {
            try{
                const {data} = await this.axios.put('http://localhost:3000/products/'+this.id, this.postdata)
                console.log(data)
                Swal.fire({
                    icon: 'success',
                    title: 'Update Success',
                });
                this.getData()
                this.closeData()
            } catch(error) {
                console.log(error)
                alert('error')
            }
        },
        async saveDelete (item) {
            try {
                if(confirm('delete it?')) {
                    const {data} = await this.axios.delete('http://localhost:3000/products/'+item)
                    console.log(data)
                    Swal.fire({
                        icon: 'success',
                        title: 'Delete success',
                    });
                    this.getData()
                    this.closeData()
                }
            } catch (error) {
                console.log(error)
                alert('error')
            }
        }
    }
}
</script>

<style scoped>
    .manage_product_title {
        text-align: center;
        margin: 30px 0;
    }

    .block-title {
        background-color: #3b3935;
    }

    .main {
        padding: 20px 50px;
    }

    .product-name {
        text-align: center;
        background-color: rgb(219, 219, 196);
        padding: 10px 0;
    }
</style>