<template>
  <div class="edit container">
      <alert v-if="alert" :message="alert"></alert>
    <h1 class="page-header">编辑用户</h1>
    <form v-on:submit="updateCustomer">
        <div class="well">
            <h4>用户信息</h4>
            <div class="form-group">
                <label>姓名</label>
                <input type="text" class="form-control" placeholder="name" v-model="customer.name" />
            </div>
            <div class="form-group">
                <label>电话</label>
                <input type="text" class="form-control" placeholder="phone" v-model="customer.phone" />
            </div>
            <div class="form-group">
                <label>邮箱</label>
                <input type="text" class="form-control" placeholder="email" v-model="customer.email" />
            </div>
            <div class="form-group">
                <label>学历</label>
                <input type="text" class="form-control" placeholder="education" v-model="customer.education" />
            </div>
            <div class="form-group">
                <label>毕业学校</label>
                <input type="text" class="form-control" placeholder="graduationSchool" v-model="customer.graduationSchool" />
            </div>
            <div class="form-group">
                <label>职业</label>
                <input type="text" class="form-control" placeholder="profession" v-model="customer.profession" />
            </div>
            <div class="form-group">
                <label>个人简介</label>
                <textarea class="form-control" rows="10" v-model="customer.profile"></textarea>
            </div>
            <button type="submit" class="btn btn-primary">确认</button>
        </div>
    </form>
  </div>
</template>

<script>
import alert from "./alert"
export default {
  name: 'edit',
  data () {
    return {
      customer: {},
      alert: ''
    }
  },
  methods: {
    fetchCustomer(id){
        this.$http.get("http://localhost:3000/users/"+id)
            .then(function(response){
                // console.log(response);
                this.customer = response.body;
            });
    },
    updateCustomer(e){
        // console.log('123');
        if(!this.customer.name || !this.customer.phone || !this.customer.email){
            // console.log('请添加对应信息');
            this.alert = '请添加对应信息(姓名/电话/邮箱---必填)';
        }else{
            let updateCustomer = {
                name: this.customer.name,
                phone: this.customer.phone,
                email: this.customer.email,
                education: this.customer.education,
                graduationSchool: this.customer.graduationSchool,
                profession: this.customer.profession,
                profile: this.customer.profile
            }
            this.$http.put("http://localhost:3000/users/"+this.$route.params.id, updateCustomer)
                .then(function(response){
                    // console.log(response);
                    this.$router.push({path: '/', query: {alert: "用户信息更新成功"}});
                })
        }
        e.preventDefault();
    }
  },
  created(){
      this.fetchCustomer(this.$route.params.id);
  },
  components: {
      alert
  }
}
</script>

<style>

</style>
