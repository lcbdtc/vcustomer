<template>
  <div class="customers container">
    <alert v-if="alert" v-bind:message="alert"></alert>
    <h1 class="page-header">用户管理系统</h1>
    <input type="text" class="form-control" placeholder="搜索" v-model="filterInput">
    <br>

    <table class="table table-stripd">
      <thead>
      <tr>
        <th>姓名</th>
        <th>电话</th>
        <th>邮箱</th>
        <th></th>
      </tr>
      </thead>

      <tbody>
      <tr v-for="customer in filterBy(customers,filterInput)">
        <td>{{customer.name}}</td>
        <td>{{customer.phone}}</td>
        <td>{{customer.email}}</td>
        <td>
          <router-link class="btn btn-default" v-bind:to="'/customer/' + customer.id">详情</router-link>

        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import Alert from './Alert'
  import CustomerDetail from './CustomerDetail'
  import Edit from './Edit'
    export default {
        name: "customers",
      data(){
          return {
            customers:[],
            alert:"",
            filterInput:""
          }
      },
      methods:{
          fetchCustomers(){
            this.$http.get('http://localhost:3000/users')
              .then((response) =>{
                // console.log(response)
                this.customers = response.data
              })
          },
        filterBy(customers,value){
            return customers.filter(function(customer){
              return customer.name.match(value) //返回整个对象
            })
        }
      },
      created:function(){
          if(this.$route.query.alert){
            this.alert = this.$route.query.alert
          }
          this.fetchCustomers()
      },
      components:{
          Alert,
        CustomerDetail,
        Edit
      }
    }
</script>

<style scoped>

</style>
