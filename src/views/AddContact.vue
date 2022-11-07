<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">Add Contact</p>
                <p class="fst-italic">ทำงานเชิงรุก (Proactive) ไม่ใช่รอรับงาน (Reactive) ผู้จัดการต้องเป็นผู้กำหนดเป้าหมายให้กับทีมงานแล้วลงมือปฏิบัติตามแผนงาน เพื่อให้บรรลุผลสำเร็จ ไม่ใช่รอรับงานเหมือในอดีตอีกต่อไป อีกทั้งยังต้องพัฒนาให้ทีมงานทำงานเชิงรุกด้วย โดยการจูงใจให้ทุกคนมีเป้าหมาย ของตัวเองแต่ต้องสอดคล้องกับเป้าหมาย ของทีมงานด้วย และให้ทุกคนวางแผนงานของตัวเองให้ละเอียด แล้วจึงปฏิบัติงานตามแผนอย่างต่อเนื่องต่อไป</p>
            </div>
        </div>
    </div>
    <div class="container mt-3">
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="submitCreate()" >
                    <div class="mb-2">
                        <input required type="text" class="form-control" placeholder="Name" v-model="contact.name">
                    </div>
                    <div class="mb-2">
                        <input required type="text" class="form-control" placeholder="Photo URL" v-model="contact.photo">
                    </div>
                    <div class="mb-2">
                        <input required type="email" class="form-control" placeholder="Email" v-model="contact.email">
                    </div>
                    <div class="mb-2">
                        <input required type="number" class="form-control" placeholder="Mobile" v-model="contact.mobile">
                    </div>
                    <div class="mb-2">
                        <input required type="text" class="form-control" placeholder="Company" v-model="contact.company">
                    </div>
                    <div class="mb-2">
                        <input required type="text" class="form-control" placeholder="Title" v-model="contact.title">
                    </div>
                    <div class="mb-2">
                        <select class="form-control" v-model="contact.groupId"  v-if="groups.length > 0">
                            <option value="">Select Group</option>
                            <option :value="groupItem.id" v-for="(groupItem) of groups" :key="groupItem.id">{{groupItem.name}}</option>
                        </select>
                    </div>
                    <div class="mb-2">
                        <input type="submit" class="btn btn-success" value="Create">
                    </div>
                </form>
            </div>
            <div class="col-md-4">
                <img :src="contact.photo" alt="" class="contact-img">
            </div>
        </div>
    </div>
</template>

<script >
import {ContactService} from '../services/ContactService'

export default {
    name:"AddContact",
    data: function(){
        return{
            contact :{
                name: "",
                photo: "",
                email: "",
                mobile: "",
                company: "",
                title: "",
                groupId: "",
            },
            groups : [],
        }
    },
    created : async function () {
    try{

        let response = await ContactService.getAllGroup();
        this.groups = response.data;

    }
    catch(error){
        console.log(error)
    }
  },
  methods:{
    submitCreate(){
        try{
            const response = ContactService.createContact(this.contact)
            if(response){
                return this.$router.push('/');
            }else{
                return this.$router.push('/contacts/add');
            }
        }
        catch(err){
            console.log(err)
        }
    }
  }
}
</script>

<style scoped>

</style>
