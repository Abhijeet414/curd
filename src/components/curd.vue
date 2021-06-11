<template>
   <div>
 
  <table border='1' width='80%' style='border-collapse: collapse;'>
   <tr>
     <th>Username</th>
     <th>Name</th>
     <th>Email</th>
     <th></th>
   </tr>

   <!-- Add -->
   <tr>
     <td><input type='text' v-model='username'></td>
     <td><input type='text' v-model='name'></td>
     <td><input type='text' v-model='email'></td>
     <td><input type='button' value='Add' @click='addRecord();'></td>
   </tr>

   <!-- Update/Delete -->
   <tr v-for='(user,index) in users' v-bind:key="user.id">
     <td><input type='text' v-model='user.username' ></td>
     <td><input type='text' v-model='user.name' ></td>
     <td><input type='text' v-model='user.email' ></td>
     <td><input type='button' value='Update' @click='updateRecord(index,user.id)'>&nbsp;
     <input type='button' value='Delete' @click='deleteRecord(index,user.id)'></td>
   </tr>
  </table>
 
</div>
	
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios,axios)
export default {

 data() {
    return {
	users:"",
	username:"",
	name:"",
	email:""

		
	}
     },
methods: {
   allRecords: function(){
     Vue.axios.get("http://localhost:3000/users")
     .then((response)=> {
       this.users = response.data;
     })
 
   },
   addRecord: function(){

     if(this.username != '' && this.name != '' && this.email != ''){
       axios.post('http://localhost:3000/users', {
        
         username: this.username,
         name: this.name,
         email: this.email
       })
       .then(function (response) {

         // Fetch records
         this.allRecords();

         // Empty values
         this.username = '';
         this.name = '';
         this.email = '';
 
         alert(response.data);
       })
       
     }else{
       alert('Fill all fields.');
     }
 
   },
   updateRecord: function(index,id){

     // Read value from Textbox
	 var username = this.users[index].username;
     var name = this.users[index].name;
     var email = this.users[index].email;

     if(name !='' && email !='' && username !=''){
       axios.post('http://localhost:3000/users', {
         username:username,
         name: name,
         email: email
       })
       .then(function (response) {
         alert(response.data);
       })
       .catch(function (error) {
         console.log(error);
       });
     }
   },
   deleteRecord: function(index, id){
 
     axios.delete('http://localhost:3000/users', +id)
     .then(function (response) {

       // Remove index from users
       this.users.splice(index, 1);
       alert(response.users);
     })
     
    } 
  },
  created: function(){
    this.allRecords();
  }
}
</script>

<style lang="scss" scoped>

</style>