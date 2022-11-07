<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">Edit Contact</p>
                <p class="fst-italic"> สร้างทีมงานให้เป็นทีมเวิร์ค ไม่ใช่ให้ทะเลาะกัน ผู้จัดการหลายคนไม่ได้ทันระวังตัว ว่าได้สร้างความระแวงให้กับทีมงานเพราะชอบให้คนนั้นตรวจสอบคนนี้ แล้วให้คนนี้ตรวจสอบคนอื่นต่อไป เพื่อจะได้ข้อมูล แต่จริงๆ แล้วผู้จัดการ ควรเป็นผู้พูดคุยกับบุคลากรภายในทีมด้วยตัวเอง ไม่จำเป็นต้องถามคนอื่น เพราะการพูดคุยกันในทีมย่อมสร้างความเข้าใจและความ ไว้วางใจให้เกิดขึ้นภายในทีมได้ การทำงานก็จะราบรื่นไม่ต้องคิดกันไปเอง สุดท้ายทีมงานก็จะกลายเป็นทีมเวิร์ค</p>
            </div>
        </div>
    </div>
    <div class="container mt-3" >
        <div class="row">
            <div class="col-md-4">
                <form @submit="updateSubmit()" >
                    <div class="mb-2">
                        <input v-model="contactmanager.name" type="text" class="form-control" placeholder="Name" >
                    </div>
                    <div class="mb-2">
                        <input v-model="contactmanager.photo" type="text" class="form-control" placeholder="Photo URL">
                    </div>
                    <div class="mb-2">
                        <input v-model="contactmanager.email" type="email" class="form-control" placeholder="Email" >
                    </div>
                    <div class="mb-2">
                        <input v-model="contactmanager.mobile" type="number" class="form-control" placeholder="Mobile" >
                    </div>
                    <div class="mb-2">
                        <input v-model="contactmanager.company" type="text" class="form-control" placeholder="Company">
                    </div>
                    <div class="mb-2">
                        <input v-model="contactmanager.title" type="text" class="form-control" placeholder="Title">
                    </div>
                    <div class="mb-2">
                        <select class="form-control" v-model="contactmanager.groupId"  v-if="groups.length > 0">
                            <option value="">Select Group</option>
                            <option  :value="groupItem.id" v-for="(groupItem) of groups" :key="groupItem.id">{{groupItem.name}}</option>
                        </select>
                    </div>
                    <div class="mb-2">
                        <input type="submit" class="btn btn-success" value="Update">
                    </div>
                </form>
            </div>
            <div class="col-md-4">
                <img :src="contactmanager.photo" alt="" class="contact-img">
            </div>
        </div>
    </div>
</template>

<script >
import {ContactService} from '../services/ContactService'
export default {
    name:"EditContact",
    data: function(){
        return{
            contactId : this.$route.params.contactId,
            contactmanager : {
                name: "",
                photo: "",
                email: "",
                mobile: "",
                company: "",
                title: "",
                groupId: "",
            },
            groups : [],
            loading: false,
            errorMessage: null
        }
    },
    created : async function () {
    try{
        this.loading = true
        let response = await ContactService.getContact(this.contactId);
        this.contactmanager = response.data;
        let responsegroup = await ContactService.getAllGroup();
        this.groups = responsegroup.data;
        this.loading = false

    }
    catch(error){
        this.errorMessage = error
        this.loading = false
    }
  },
  methods:{
    updateSubmit(){
        try{
            const response = ContactService.updateContact(this.contactmanager, this.contactId)
            if(response){
                return this.$router.push('/');
            }else{
                return this.$router.push(`/contacts/edit/${this.contactId}`);
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
