<template>
  <div>
    <h2>ADD USER</h2>
    <form>
        <div class="Add User">
          Nama: <input type="name" name="name" placeholder="Name" v-model="nameInput" /><br />
          Email: <input type="email" name="email" placeholder="Email" v-model="emailInput" /><br />
          Password <input type="password" name="password" placeholder="Password" v-model="passwordInput" /><br />
          Role: <input type="text" name="role" placeholder="role" v-model="roleInput" /><br />
          groupName: <input type="groupName" name="groupName" placeholder="groupName" v-model="groupNameInput" /><br />
          <button @click.prevent="AddUserHandler">Simpan</button>
     </div>
  </form>
  </div>
</template>

<script>

  export default {
    data: function() {
      return { 
        nameInput:'',
        emailInput: '',
        passwordInput: '',
        roleInput:'',
        groupNameInput:''
      }
    },
    methods: {
        getNewToken(callback){
          let tokenMap={
            "token":localStorage.getItem("accessToken"),
            "refreshToken":location.getItem("refreshToken")
          }
            fetch(`http://localhost:8088/auth/refreshtoken`, {
            method: "POST",
            headers: {'Authorization': localStorage.getItem('accessToken')},
            body : formData
          })
          .then(res => {
                // this.$store.dispatch("login", res.data);
                localStorage.setItem('accessToken', `Token ${res.data.token}`);
                localStorage.setItem('refreshToken', res.data.refreshToken);
                })
                callback()
          .catch(err => {alert(`Catch an error: ${err}`)})
        
          }
        },

        AddUserHandler() {
        let AddUser = {
          name: this.nameInput,
          email: this.emailInput,
          password: this.passwordInput,
          role: this.roleInput,
          groupName:this.groupNameInput
        };
        let formData = new FormData()
        let userJSON = JSON.stringify(AddUser)
        formData.append('user',userJSON)
       fetch(`http://localhost:8088/api/user`, {
          method: "POST",
          headers: {'Authorization': localStorage.getItem('accessToken')},
          body : formData
        })
         .then(res => {
            console.log(Response)
            alert('User berhasil di simpan')
            if(!res.ok){
              this.getNewToken(AddUserHandler)
            }
            localStorage.setItem('accessToken',res.headers.get("Authorization")) //timpa acessToken di localstorage dengan yang baru setiap res.ok berhasil
          })
          .catch(err => {alert(`Catch an error: ${err}`)})  
    },
    
  }
</script>
