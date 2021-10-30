<template>
    <div class="container">
    <div class="tab-pane p-3 fade show active">
      <div class="row">
        <div class="col-md-12">
          <h4>Users Table</h4>
          <table class="table">
            <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">Email</th>
                <th scope="col"></th>
              </tr>
            </thead>
            <tbody>
              <template v-for="(user, index) in users" i = index>
                <tr :key="user.id">
                  <th scope="row">{{ index + 1 }}</th>
                  <td>{{ users }}</td>
                  <!-- <td v-if="invoice.paid == 0">Unpaid</td> -->
                  <!-- <td v-else>Paid</td> -->
                  <!-- <td><a href="./SingleInvoice" class="btn btn-success">To Invoice</a></td> -->
                </tr>
              </template>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
    name: "Users",
    data() {
        return {
            users: [],
        };
    },
    mounted() {
        axios
            .get(`http://10.30.30.26:9000/api/users/fetch`,
            { headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`}})
            .then(res => {
                if (res.data.status == 'success') {
                   
                    this.users = res.data.data.userMap[0];                  
                     console.log(this.users)
                }
            });
    }
};
</script>