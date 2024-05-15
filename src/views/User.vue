<template>
  <div>
    <v-card width="95%">
<v-simple-table>
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">Username</th>
          <th class="text-left">First Name</th>
          <th class="text-left">Last Name</th>
          <th class="text-left">E-mail</th>
          <th class="text-left">Approve</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in apidata" :key="index">
          <td>{{ item.username }}</td>
          <td>{{ item.first_name }}</td>
          <td>{{ item.last_name }}</td>
          <td>{{ item.email }}</td>
          <td><v-btn>{{ item.approve }}</v-btn></td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
    </v-card>
  </div>
</template>

<script>
export default {
    data () {
        return {
            apidata: []
        }
    },


    methods: {
        created () {
            this.getData()
        },

        getData() {
            this.axios.get('http://localhost:3000/users')
                .then((response) => {
                    console.log('response: ', response)
                    this.apidata = response.data.data
                })
        },
    }
}
</script>

<style>

</style>