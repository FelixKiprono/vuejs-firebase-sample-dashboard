<template>
  <div class="login_div">
    <a-form
      id="login"
      class="login-form2"
      :model="form"
      @submit="handleSubmit"
      @submit.native.prevent
    >
      <a-divider>Admin Login</a-divider>
      <a-form-item>
        <a-input placeholder="Username" v-model="form.username">
          <a-icon slot="prefix" type="user" style="color: rgba(0,0,0,.25)" />
        </a-input>
      </a-form-item>
      <a-form-item>
        <a-input type="password" v-model="form.password" placeholder="Password">
          <a-icon slot="prefix" type="lock" style="color: rgba(0,0,0,.25)" />
        </a-input>
      </a-form-item>
      <a-form-item>
        <a-checkbox
          v-decorator="[
            'remember',
            {
              valuePropName: 'checked',
              initialValue: true
            }
          ]"
        >
          Remember me </a-checkbox
        ><br />

        <a-button type="primary" html-type="submit" class="login-form-button">
          Log in
        </a-button>
        <br />

        <a class="login-form-forgot" href="">
          Forgot password
        </a>
        or you can
        <a href="">
          register now!
        </a>

        <a-alert message="" v-if="visible" closable type="error">
          <p slot="description">
            <span style="color: red">
              {{ error }}
            </span>
          </p>
        </a-alert>
      </a-form-item>
    </a-form>
  </div>
</template>

<script>
import * as firebase from "firebase/app";
import "firebase/firestore";

export default {
  //beforeCreate()
  //{
  //this.form = this.$form.createForm(this, { name: 'normal_login' });
  //},
  data() {
    return {
      error: "",
      visible: false,
      form: {
        username: "",
        password: ""
      }
    };
  },

  methods: {
    handleSubmit(e) {
      var StoreDB = firebase.firestore();

      var Users = [];

      try {
        StoreDB.collection("users")
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
             // Users.push({username:doc.data().username,password:doc.data().password});            
              console.log('Server '+doc.data().username +' local uid '+this.form.username +' Passes '+doc.data().password +' [ '+this.form.password);
              if (
                doc.data().username === this.form.username &&
                doc.data().password.toString() === this.form.password.toString()
              ) {
                this.visible = false;
                localStorage.setItem('IsLogged',true);
                localStorage.setItem("account", doc.data().accounttype);
                this.$router.replace({ name: "admin" });
                return;
              }
              //  console.log(`${doc.id} => ${doc.data().username}`);
            });
            this.visible = true;
            this.error = "Sorry Your details are incorect";
             //localStorage.setItem('IsLogged',false);
            return;
          });
      } catch {}
      //do our users query
      // console.log(Users);
      // let User = Users.filter(function(item)
      // {
      //  if(item.username===this.form.username && item.password.toString()===this.form.password.toString())
      //  {
      //    return item;
      //  }
      // });
      //  console.log(User);
      e.preventDefault();
    }
  },
  created()
  {
   
  }
};
</script>

<style>
.login_div {
  position: absolute;

  width: 400px;
  height: 400px;

  /* Center form on page horizontally & vertically */
  top: 50%;
  left: 50%;
  margin-top: -150px;
  margin-left: -150px;
}

.login_form2 {
  width: 400px;
  height: 400px;

  background: gray;
  border-radius: 10px;

  margin: 0;
  padding: 0;
}
</style>
