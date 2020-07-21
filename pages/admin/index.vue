<template>
  <a-layout id="components-layout-demo-side" style="min-height: 100vh">
    <a-layout-sider 
    v-model="collapsed" 
    :mode="mode" 
    :theme="theme" 
    collapsible>
      <div class="logo"></div>      
     

      <a-menu @click="handleClick" :mode="mode" :theme="theme" :default-selected-keys="['1']">

        <a-menu-item key="admin">
          <a-icon type="safety" />
          <span>Admin</span>
        </a-menu-item>
        
        <a-sub-menu key="sub2" >         
          <span slot="title">
            <a-icon type="team" />
            <span >Beneficiaries</span>
          </span>
           <a-menu-item key="new">
            Add New 
          </a-menu-item>
          <a-menu-item key="view">
            View 
          </a-menu-item>
         
        </a-sub-menu>    
       
     

         <a-menu-item key="logout">
          <a-icon type="logout" />
          <span>Logout</span>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>

    <a-layout>
      <a-layout-header style="background: #fff; padding:0">
        <h2 style="margin-left:25px;">Scholarship Disbursement </h2>
 
      

      </a-layout-header>
     

      <a-layout-content style="margin: 0 16px;">
        <a-breadcrumb style="margin: 16px 0"> </a-breadcrumb>
        <div
          :style="{
            padding: '24px',
            background: '#fff',
            height: '100%',
            width: '100%'
          }"
        >
        <component v-bind:is="component"> </component>
        </div>
      </a-layout-content>
      <a-layout-footer style="text-align: center">
        All rights reserved copyright ©2020
      </a-layout-footer>
    </a-layout>
  </a-layout>
</template>
<script>

import NewMember from '~/components/newmember'
import ViewMember from '~/components/viewmembers'
import * as firebase from 'firebase/app'

export default {
    components:{newmember:NewMember,viewmember:ViewMember},
   data() {
    return {
      component: "viewmember",
      collapsed: false,
      IsAdmin:false,
      //global title
      Title: "",
      // theming
      mode: "inline",
      theme: "dark"
    };
  },
  methods: {

  
    handleClick(menu)
    {
      //load page based on the menu clicked
     switch(menu.key)
     {      
        case 'admin':
          this.component = '';
         break;

       case 'new':
         if(this.IsAdmin)
         {
         this.component = 'newmember';
         }
         else{
               this.$message.info("Sorry you cannot access this page.");
           //alert('Sorry you cannot access this page');
         }
         break;

           case 'view':
         this.component = 'viewmember';
         break;
         case 'logout':
           this.$message.info("Redirecting your to login page ...");
            localStorage.removeItem('IsLogged');
       localStorage.removeItem('account');
       this.$router.push("/");

           break;
         
     }
      //console.log('click ', menu);
     },  
    changeMode(checked) 
    {
      this.mode = checked ? "vertical" : "inline";
    },
    changeTheme(checked) 
    {
      this.theme = checked ? "light" : "dark";
    },
    logout()
    {
      localStorage.removeItem('IsLogged');
       localStorage.removeItem('account');
       this.$router.push("/");

    }
  },
  created() 
  {
    //check if it is logged in
     var islogin= localStorage.getItem('IsLogged');
     
     if(!islogin)
     { 
       this.$router.push("/");
       return;
     }
    //check previleges
    var acctype= localStorage.getItem('account').toLowerCase();
    
    if(acctype==='superadmin')
    {
      this.IsAdmin=true;
    }
    else
    {
      this.IsAdmin=false;
    }
    
    
   

  }
};
</script>
<style>
    #components-layout-demo-side .logo {
      height: 32px;
      background: rgba(255, 255, 255, 0.2);
      margin: 16px;
      }
</style>
