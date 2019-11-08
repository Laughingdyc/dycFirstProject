<template>
  <div class="customers container">
    <alert v-if="alert" :message="alert"></alert>
    <h1 class="page-header">用户管理系统</h1>
    <div>
      <input type="text" class="form-control" placeholder="搜索" v-model="filterInput"><br>
    </div>
    <table class="table table-striped">
        <thead>
            <tr>
                <th>姓名</th>
                <th>电话</th>
                <th>邮箱</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="customer in filterBy(customers, filterInput)" >
                <td>{{customer.name}}</td>
                <td>{{customer.phone}}</td>
                <td>{{customer.email}}</td>
                <td><router-link class="btn btn-default" :to="'/customer/'+customer.id">详情</router-link></td>
            </tr>
        </tbody>
    </table>

  </div>
</template>

<script>
import alert from "./alert"
export default {
  name: 'customers',
  data () {
    return {
      customers: [],
      alert: '',
      filterInput: ''
    }
  },
  methods: {
      fetchCustomers(){
          this.$http.get("http://localhost:3000/users").then(function(response){
                //   console.log(response.body);
                this.customers = response.body;
              })
      },
      filterBy(customers, value){
        return customers.filter(function(customer){
          return customer.name.match(value);
        })
      }
  },
  created(){
    if(this.$route.query.alert){
      this.alert = this.$route.query.alert;
    }
      this.fetchCustomers();
  },
  updata(){
      this.fetchCustomers();
  },
  components: {
    alert
  }
}
</script>
<style>
</style>
