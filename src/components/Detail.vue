<template>
  <div class="card" style="background-color:#8398bf">
        <div class="card-body">
            <!-- Formบันทึกข้อมูล -->
            <form @submit.prevent="handleSubmit()">
                <div class="row">
                    <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                        <label for="stdId" class="form-label">Name</label>
                        <input type="text" id="stdId" class="form-control" v-model.trim="major.id" />
                    </div>
                    <div class="col-lg-8 col-md-8 col-sm-6 mt-2">
                        <label for="stdName" class="form-label">ID</label>
                        <input type="text" id="stdName" class="form-control" v-model.trim="major.name" />
                    </div>
                    <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                        <label for="" class="form-label">NO</label>
                        <input type="text" id="stdName" class="form-control" v-model.trim="major.credit" />
                    </div>
                    <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                        <label for="stdYr" class="form-label">School</label>
                        <input type="text" id="stdName" class="form-control" v-model.trim="major.grade" />
                    </div>
                    <div class="col-2 mt-4 mx-4 d-flex align-item-center">
                        <button type="submit" class="btn btn-warning">Save</button>
                    </div>
                </div>
                <div class="alert alert-success mt-5" v-if="editOK">
                    {{ editMessage }}
                </div>
                <div class="alert alert-danger mt-5" v-if="editErr">
                    {{ editMessage }}
                </div>
            </form>
        </div>
    </div>
    <div class="card" style="background-color:#8398bf">
        <div class="card-body">
            <!-- Formบันทึกข้อมูล -->
            <form @submit.prevent="handleSubmit()">
                <div class="row">
                    <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                        <label for="stdId" class="form-label">Course ID</label>
                        <input type="text" id="stdId" class="form-control" v-model.trim="major.id" />
                    </div>
                    <div class="col-lg-8 col-md-8 col-sm-6 mt-2">
                        <label for="stdName" class="form-label">Course Name</label>
                        <input type="text" id="stdName" class="form-control" v-model.trim="major.name" />
                    </div>
                    <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                        <label for="" class="form-label">Credit</label>
                        <input type="text" id="stdName" class="form-control" v-model.trim="major.credit" />
                    </div>
                    <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                        <label for="stdYr" class="form-label">Grade</label>
                        <input type="text" id="stdName" class="form-control" v-model.trim="major.grade" />
                    </div>
                    <div class="col-2 mt-4 mx-4 d-flex align-item-center">
                        <button type="submit" class="btn btn-warning">Save</button>
                    </div>
                </div>
                <div class="alert alert-success mt-5" v-if="editOK">
                    {{ editMessage }}
                </div>
                <div class="alert alert-danger mt-5" v-if="editErr">
                    {{ editMessage }}
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import { EventBus } from '@/event-bus';
export default {
    name:"EditName",
    props:["NameID"],
    data(){
        return{
            name:{
                Name: null,
                ID: null,
                NO: null,
                School : null,
                isActive : false
            },
            editOK : false,
            editErr : false,
            editMessage : null
        }
    },
    mounted(){
        fetch(`http://localhost:3000/Default/${this.MajorID}`)
        .then((res) => res.json())
        .then((data) => {
            this.name.ID = data.ID
            this.name.Name = data.Name
            this.name.NO = data.NO
            this.name.School = data.School
        })
        .catch
    },
    methods:{
        handleSubmit(){
            fetch(`http://localhost:3000/Default/${this.MajorID}`,{
                method : "PATCH",
                headers : {"Content-Type" : "application/json"},
                body : JSON.stringify(this.subject)
            })
                .then(() => {
                    this.editOK = true;
                    this.editMessage = "Edit&Save Successfully"
                    EventBus.emit("DetailChange",{messsage:'edit'})
                })
                .catch((err) => {
                    this.editErr = true;
                    this.editOK = false;
                    this.editMessage = err;
                });
        }
    },
    name:"EditScore",
    props:["MajorID"],
    data(){
        return{
            major:{
                id: null,
                name: null,
                credit: null,
                grade : null,
                isActive : false
            },
            editOK : false,
            editErr : false,
            editMessage : null
        }
    },
    mounted(){
        fetch(`http://localhost:3000/subject/${this.MajorID}`)
        .then((res) => res.json())
        .then((data) => {
            this.major.id = data.id
            this.major.name = data.name
            this.major.credit = data.credit
            this.major.grade = data.grade
        })
        .catch
    },
    methods:{
        handleSubmit(){
            fetch(`http://localhost:3000/subject/${this.MajorID}`,{
                method : "PATCH",
                headers : {"Content-Type" : "application/json"},
                body : JSON.stringify(this.subject)
            })
                .then(() => {
                    this.editOK = true;
                    this.editMessage = "Edit&Save Successfully"
                    EventBus.emit("MajorChange",{messsage:'edit'})
                })
                .catch((err) => {
                    this.editErr = true;
                    this.editOK = false;
                    this.editMessage = err;
                });
        }
    }
}
</script>

<style>

</style>