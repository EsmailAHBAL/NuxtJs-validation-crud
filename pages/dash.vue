<template>
  <div class="container" style="margin-top: 5%;">
    <div class="" style="display: flex;justify-content: space-around;">
      <strong>Welcome</strong> <button class="button is-danger" @click="logout()">Danger</button>

    </div>
    <hr />
    <div class="box">
      <span class="tag is-primary">Email</span> {{user.email}}
      <hr />
      <span class="tag is-primary">Username</span> {{user.username}}
    </div>
    <hr />
    <div style="width: 30%;">
        <h1>Add Note</h1>
    <form @submit.prevent="addNote()">
      <div class="field">
        <p class="control has-icons-left has-icons-right">
          <input 
          v-model="note.title"
          class="input" type="text" placeholder="Title">
          <span class="icon is-small is-left">
            <i class="fas fa-envelope"></i>
          </span>
          <span class="icon is-small is-right">
            <i class="fas fa-check"></i>
          </span>
        </p>
      </div>
         <div class="field">
        <p class="control has-icons-left has-icons-right">
          <textarea   
          class="textarea is-info"
          v-model="note.note">

          </textarea> 
        
        </p>
      </div>
      <div>
        <button class="button is-info" type="submit"> Add Note </button>
      </div>
    </form>
    </div>
    <hr/>
    <div>
        <div class="box" 
        style="display: flex;justify-content: space-between;"
        v-for="note in notes">
        {{note.title}} 
          {{note.note}} 

          <span class="tag is-danger"
          @click="show(note)"
          >X</span>
        </div>
    </div>
  </div>
</template>
<script>
  export default {
    data: () => ({
      user: {}
      ,note:{
        title:"",
        note:""
      }
      ,notes:[]
    }),
    mounted() {
      this.getData();
    },
    methods: {
      getData() {

        this.user = JSON.parse(localStorage.getItem("user"));
      },
      logout() {
        alert("log out");
        localStorage.removeItem("user");
        this.$router.push("/")
      },
      addNote(){
        this.notes.push(this.note);
        this.note={}
      },
      show(p){
       var removeItem=this.notes.filter(
        el=>{
              if(el.title != p.title){
                return el;
              }
        }
       );
       this.notes=removeItem;

      }
    }
  }

</script>
