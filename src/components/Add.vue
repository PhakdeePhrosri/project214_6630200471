<template>
    <div class="card" style="background-color:#8398bf">
      <div class="card-body">
        <!-- แสดงข้อมูลนิสิต -->
        <div class="text-center">
          <img :src="student.image" alt="Student Image" class="rounded-circle" width="150" height="150" />
          <h4 class="mt-2">{{ student.fullName }}</h4>
          <p><strong>Student ID:</strong> {{ student.id }}</p>
          <p><strong>Major:</strong> {{ student.major }}</p>
          <p><strong>Previous School:</strong> {{ student.previousSchool }}</p>
          <button class="btn btn-primary" @click="isEditing = true">Edit</button>
        </div>
        
        <!-- ฟอร์มแก้ไขข้อมูลนิสิต -->
        <form v-if="isEditing" @submit.prevent="handleSubmit">
          <div class="row">
            <div class="col-md-6 mt-2">
              <label class="form-label">Full Name</label>
              <input type="text" class="form-control" v-model.trim="student.fullName" />
            </div>
            <div class="col-md-6 mt-2">
              <label class="form-label">Student ID</label>
              <input type="text" class="form-control" v-model.trim="student.id" />
            </div>
            <div class="col-md-6 mt-2">
              <label class="form-label">Major</label>
              <input type="text" class="form-control" v-model.trim="student.major" />
            </div>
            <div class="col-md-6 mt-2">
              <label class="form-label">Previous School</label>
              <input type="text" class="form-control" v-model.trim="student.previousSchool" />
            </div>
            <div class="col-md-12 mt-3">
              <button type="submit" class="btn btn-success">Save</button>
              <button type="button" class="btn btn-secondary ms-2" @click="isEditing = false">Cancel</button>
            </div>
          </div>
        </form>
        
        <div class="alert alert-success mt-3" v-if="updateSuccess">{{ updateMessage }}</div>
        <div class="alert alert-danger mt-3" v-if="updateError">{{ updateMessage }}</div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "StudentInfo",
    data() {
      return {
        student: {
          image: "https://via.placeholder.com/150",
          fullName: "John Doe",
          id: "12345678",
          major: "Computer Science",
          previousSchool: "ABC High School"
        },
        isEditing: false,
        updateSuccess: false,
        updateError: false,
        updateMessage: ""
      };
    },
    methods: {
      handleSubmit() {
        fetch(`http://localhost:3000/Default/${this.student.id}`, {
          method: "PUT",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(this.student)
        })
          .then(() => {
            this.updateSuccess = true;
            this.updateMessage = "Updated Successfully";
            this.isEditing = false;
          })
          .catch((err) => {
            this.updateError = true;
            this.updateMessage = "Update Failed: " + err;
          });
      }
    }
  };
  </script>
  
  <style>
  img {
    border: 3px solid white;
  }
  </style>
  