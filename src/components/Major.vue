<template>
    <div class="card">
      <div class="card-body">
        <h3 class="text-center">📚 รายละเอียดรายวิชา</h3>
        <table class="table table-bordered mt-3">
          <thead>
            <tr>
              <th>Course ID</th>
              <th>Course Name</th>
              <th>Credit</th>
              <th>Grade</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="subject in subjects" :key="subject.id">
              <td>{{ subject.id }}</td>
              <td>{{ subject.name }}</td>
              <td>{{ subject.credit }}</td>
              <td>{{ subject.grade }}</td>
              <td>
                <button class="btn btn-primary btn-sm" @click="editSubject(subject)">Edit</button>
                <button class="btn btn-danger btn-sm mx-1" @click="deleteSubject(subject.id)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
  
        <h4 class="mt-4">{{ isEditing ? "✏️ Edit Subject" : "➕ Add New Subject" }}</h4>
        <form @submit.prevent="handleSubmit">
          <div class="row">
            <div class="col-md-4">
              <label class="form-label">Course ID</label>
              <input type="text" class="form-control" v-model="major.id" :disabled="isEditing" required />
            </div>
            <div class="col-md-4">
              <label class="form-label">Course Name</label>
              <input type="text" class="form-control" v-model="major.name" required />
            </div>
            <div class="col-md-2">
              <label class="form-label">Credit</label>
              <input type="number" class="form-control" v-model="major.credit" required />
            </div>
            <div class="col-md-2">
              <label class="form-label">Grade</label>
              <input type="text" class="form-control" v-model="major.grade" required />
            </div>
          </div>
  
          <button type="submit" class="btn btn-success mt-3">{{ isEditing ? "Save Changes" : "Add Subject" }}</button>
          <button type="button" class="btn btn-secondary mt-3 mx-2" v-if="isEditing" @click="cancelEdit">Cancel</button>
        </form>
  
        <div v-if="message" :class="['alert', alertType, 'mt-3']">{{ message }}</div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        subjects: [],
        major: { id: "", name: "", credit: "", grade: "" },
        isEditing: false,
        message: "",
        alertType: "alert-success"
      };
    },
    mounted() {
      this.fetchSubjects();
    },
    methods: {
      async fetchSubjects() {
        try {
          const res = await fetch("http://localhost:3000/subjects");
          this.subjects = await res.json();
        } catch (error) {
          console.error("Error fetching subjects:", error);
        }
      },
      async handleSubmit() {
        try {
          if (this.isEditing) {
            // อัปเดตข้อมูล
            await fetch(`http://localhost:3000/subjects/${this.major.id}`, {
              method: "PUT",
              headers: { "Content-Type": "application/json" },
              body: JSON.stringify(this.major)
            });
            this.message = "✅ รายวิชาถูกแก้ไขเรียบร้อยแล้ว!";
          } else {
            // เพิ่มข้อมูลใหม่
            await fetch("http://localhost:3000/subjects", {
              method: "POST",
              headers: { "Content-Type": "application/json" },
              body: JSON.stringify(this.major)
            });
            this.message = "✅ รายวิชาถูกเพิ่มเรียบร้อย!";
          }
          this.alertType = "alert-success";
          this.resetForm();
          this.fetchSubjects();
        } catch (error) {
          this.alertType = "alert-danger";
          this.message = "❌ ไม่สามารถบันทึกข้อมูลได้!";
          console.error("Error saving subject:", error);
        }
      },
      editSubject(subject) {
        this.major = { ...subject };
        this.isEditing = true;
      },
      async deleteSubject(id) {
        if (!confirm("⚠️ คุณแน่ใจหรือไม่ว่าต้องการลบรายวิชานี้?")) return;
        try {
          await fetch(`http://localhost:3000/subjects/${id}`, { method: "DELETE" });
          this.message = "✅ รายวิชาถูกลบเรียบร้อย!";
          this.alertType = "alert-success";
          this.fetchSubjects();
        } catch (error) {
          this.alertType = "alert-danger";
          this.message = "❌ ไม่สามารถลบข้อมูลได้!";
          console.error("Error deleting subject:", error);
        }
      },
      cancelEdit() {
        this.resetForm();
      },
      resetForm() {
        this.major = { id: "", name: "", credit: "", grade: "" };
        this.isEditing = false;
      }
    }
  };
  </script>
  
  <style scoped>
  .card {
    background-color: #8398bf;
    padding: 20px;
    border-radius: 10px;
  }
  .table {
    background: white;
    border-radius: 5px;
  }
  </style>
  