<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold">
          Contact Manager
          <router-link to="/contacts/add" class="btn btn-success btn-sm"
            ><i class="fa fa-plus-circle"></i> New</router-link
          >
        </p>
        <p class="fst-italic">
          หน้าที่บริหาร, ควบคุม, จัดการ ไม่ใช่ลงมือทำงาน ผู้จัดการ
          ควรไว้วางใจให้ลูกน้องเป็นผู้ปฏิบัติงานในหน้าที่ของเขา
          ถ้าไม่จำเป็นก็ไม่ควรไปทำแทนเขา เพราะเขาก็จะไม่เก่งถ้าเราคอย
          ทำแทนเขาไปหมด เราจึงควรแค่ชี้แนะเท่านั้น ดังนั้น จึงควรกำหนดเป้าหมาย
          และบริหารให้ได้ตามเป้าหมาย
          คอยตรวจสอบและควบคุมให้งานอยู่ในแผนที่กำหนดไว้
        </p>
        <form>
          <div class="row">
            <div class="col-md-6">
              <div class="row">
                <div class="col">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Search Name"
                  />
                </div>
                <div class="col">
                  <input type="submit" class="btn btn-outline-dark" />
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>

  <!-- Sprinner -->
  <div v-if="loading">
    <div class="container">
      <div class="row">
        <div class="col">
          <Sprinner/>
        </div>
      </div>
    </div>
  </div>


  <!-- Errormessage -->
  <div v-if="!loading && errorMessage">
    <div class="container mt-3">
      <div class="row">
        <div class="col">
          <p class="h4 text-danger fw-bold">{{errorMessage}}</p>
        </div>
      </div>
    </div>
  </div>

  <div class="container mt-3" v-if="contacts.length > 0">
    <div class="row">
        <div class="col-md-6" v-for="(item,index) of contacts" :key="index">
            <div class="card my-2 list-group-item-success shadow-lg" >
                <div class="card-body">
                    <div class="row align-items-center">
                        <div class="col-sm-4">
                            <img :src="item.photo" alt="" class="contact-img">
                        </div>
                        <div class="col-sm-7">
                            <ul class="list-group">
                                <li class="list-group-item">Name : <span class="fw-bold">{{item.name}}</span></li>
                                <li class="list-group-item">Email : <span class="fw-bold">{{item.email}}</span></li>
                                <li class="list-group-item">Mobile : <span class="fw-bold">{{item.mobile}}</span></li>
                            </ul>
                        </div>
                        <div class="col-sm-1 d-flex flex-column justify-content-center align-items-center">
                            <router-link :to="`/contacts/view/${item.id}`" class="btn btn-warning my-1"><i class="fa fa-eye" ></i></router-link>
                            <router-link :to="`/contacts/edit/${item.id}`" class="btn btn-primary my-1"><i class="fa fa-pen" ></i></router-link>
                            <button class="btn btn-danger" @click="clickDeleteContact(item.id)"><i class="fa fa-trash my-1"></i></button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script >
import {ContactService} from '../services/ContactService'
import Sprinner from '../components/SpinnerLoad.vue'

export default {
  name: "ContactManager",
  components:{
    Sprinner
  },
  data : function(){
    return{
        loading : false,
        contacts: [],
        errorMessage: null,
    }
  },
  created : async function () {
    try{
        this.loading = true;
        let response = await ContactService.getAllContacts();
        this.contacts = response.data;
        this.loading = false;
    }
    catch(error){
        this.errorMessage = error
        this.loading = false
    }
  },
  
  methods : {
    clickDeleteContact(contactId){
      try{
        this.loading = true;
        let response = ContactService.deleteContact(contactId);
        if(response){
          let response = ContactService.getAllContacts(); //getting fresh data
          this.contacts = response.data;
          this.loading = false;
        }
      }
      catch(error){
        this.errorMessage = error
        this.loading = false
      }
    }
  }
};
</script>

<style scoped></style>
