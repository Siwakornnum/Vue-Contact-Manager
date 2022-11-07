<template>
     <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">View Contact</p>
                <p class="fst-italic"> เผชิญหน้ากับปัญหาร่วมกับทีมงาน ไม่ใช่แค่การรายงานผู้บริหาร ผู้จัดการมีหน้าที่รับผิดชอบปัญหาที่เกิดขึ้น ไม่ว่าจะเป็นใครในทีมงานที่ปฏิบัติงานผิดพลาด เพราะเป็นทีมงานของเรา ดังนั้นจึงควรเข้าร่วมแก้ไขปัญหาโดยการตรวจสอบ, กำหนดแนวทาง, ให้คำแนะนำและอาจร่วมช่วยเหลือถ้าจำเป็น เพื่อให้ปัญหา/อุปสรรคได้รับการแก้ไขอย่างทันท่วงที ไม่ใช่แค่คอยตอบคำถามผู้บริหารว่าเกิดอะไรขึ้น ถ้าทำอย่างนั้นแล้วคงจะได้ใจทีมงานยากครับ</p>
            </div>
        </div>
    </div>
    <div class="container"  v-if="!loading && isDone()">
        <div class="row align-items-center" >
            <div class="col-md-4">
                <img :src="contactmanager.photo" alt="" class="contact-img-big">
            </div>
            <div class="col-md-6" >
                <ul class="list-group">
                    <li class="list-group-item">Name : <span class="fw-bold">{{contactmanager.name}}</span></li>
                    <li class="list-group-item">Mobile : <span class="fw-bold">{{contactmanager.mobile}}</span></li>
                    <li class="list-group-item">Email : <span class="fw-bold">{{contactmanager.email}}</span></li>
                    <li class="list-group-item">Company : <span class="fw-bold">{{contactmanager.company}}</span></li>
                    <li class="list-group-item">Title : <span class="fw-bold">{{contactmanager.title}}</span></li>
                    <li class="list-group-item">Group : <span class="fw-bold">{{groupmanager.name}}</span></li>
                </ul>
            </div>
        </div>
        <div class="row mt-3">
            <div class="col">
                <router-link to="/contacts" class="btn btn-success"><i class="fa fa-arrow-alt-circle-left"></i> Back</router-link>
            </div>
        </div>
    </div>
</template>

<script >
import {ContactService} from '../services/ContactService'
export default {
    name:"ViewContact",
    data: function(){
        return{
            contactId : this.$route.params.contactId,
            contactmanager : [],
            groupmanager : [],
            loading: false,
            errorMessage: null
        }
    },
    created : async function () {
    try{
        this.loading = true
        let response = await ContactService.getContact(this.contactId);
        this.contactmanager = response.data;
        let responsegroups = await ContactService.getGroup(this.contactmanager);
        this.groupmanager = responsegroups.data;
        this.loading = false

    }
    catch(error){
        this.errorMessage = error
        this.loading = false
    }
  },
  methods:{
    isDone(){
        return Object.keys(this.contactmanager).length > 0 && Object.keys(this.groupmanager).length > 0 ;
    }
  }
}

</script>


<style scoped>

</style>
