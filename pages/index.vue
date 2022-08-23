<template>
  <section class="container" style="left: 20%;padding-top: 5%;">
    <form @submit.prevent="sendData()" style="width: 420px;" Method="POST">
      <div class="field">
        <label class="label">Username</label>
        <div class="control">
          <input v-model="user.username" class="input" type="text" placeholder="Text input">
        </div>
      </div>

      <div class="field">
        <label class="label">Name</label>
        <div class="control">
          <input v-model="user.email" class="input" type="text" placeholder="Text input">
        </div>
      </div>
      <div class="field is-grouped">
        <div class="control">
          <button class="button is-link" type="submit">Submit</button>
        </div>
        <div class="control">
          <button class="button is-link is-light">Cancel</button>
        </div>
      </div>
      <div v-if="errorMessage">
       <hr />
      <div>
        <div class="notification is-danger">
          <button class="delete"></button>
          {{errorMessage}}
        </div>
        </div>
      </div>
      <hr />
      <div>
        
      <div  v-for="user in users">
        <div class="box" style="display: flex;justify-content: space-around;">
      
          <div>
         <span class="tag is-primary is-light">Username </span> {{user.username}}

       </div>
       <div>

         <span class="tag is-primary is-dark"> Email </span> {{user.email}}
       </div>

        </div>
        <hr />
      </div>
      </div>
    </form>
  </section>
</template>

<script>
  import Joi from "joi";
  const shema = Joi.object({
    username: Joi.string().min(3).max(15),
    email: Joi.string().email({
      minDomainSegments: 2,
      tlds: {
        allow: ['com', 'net']
      }
    })

  })
  export default {
    name: 'IndexPage',
    data: () => ({
      user: {
        username: "",
        email: ""
      },
      users: [{
          username: "Ali",
          email: "Ali@gmail.com"
        },
        {
          username: "Saad",
          email: "Saad@gmail.com"
        },
      ],
      errorMessage: "",
      
    }),
    
    watch:{
      user :{
        handler(){
          this.errorMessage="";
        },deep:true
      }
    },
    methods: {
      sendData() {
        if (this.isValidate()) {
          var ifE = true;
          this.users.map(user => {
            if (user.email == this.user.email) {
              ifE = false;
              this.errorMessage="Email Already Exisit 📌 ";
            } else {
              ifE = true;
            }
          });
          if (ifE) {
            localStorage.user=JSON.stringify(this.user);
            this.users.push(this.user);
            this.$router.push("/dash")
            this.user = {
              username: "",
              email: ""
            }
          }
        }

      },
      isValidate() {
        const {
          value,
          error
        } = shema.validate(this.user);





        if (!error) {

          return true;

        }
        if (error.message.includes("username")) {

          this.errorMessage = error.message + " 🔒 ";
          return false;

        } else {
          this.errorMessage = error.message + " 🔒 ";
          return false;

        }


      }
    }
  }

</script>
