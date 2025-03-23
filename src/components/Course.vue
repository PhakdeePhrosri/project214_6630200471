<template>
  <div>
    <h1>จัดการรายวิชา</h1>
    <button @click="openForm()">➕ Add Course</button>

    <div class="course-list">
      <div v-for="course in courses" :key="course.id" class="course-card">
        <h3>{{ course.code }} - {{ course.name }}</h3>
        <p>หน่วยกิต: {{ course.credits }}</p>
        <p>เกรด: {{ course.grade }}</p>
        <div class="actions">
          <button @click="openForm(course)">✏️ Edit</button>
          <button @click="deleteCourse(course.id)">❌ Delete</button>
        </div>
      </div>
    </div>

    <div v-if="showForm" class="form-modal">
      <h2>{{ isEditing ? 'แก้ไข' : 'เพิ่ม' }} รายวิชา</h2>
      <form @submit.prevent="saveCourse">
        <input v-model="form.code" placeholder="รหัสวิชา" required />
        <input v-model="form.name" placeholder="ชื่อวิชา" required />
        <input v-model.number="form.credits" type="number" placeholder="หน่วยกิต" required />
        <input v-model="form.grade" placeholder="เกรด" required />
        <button type="submit">บันทึก</button>
        <button @click="closeForm">ยกเลิก</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      courses: [],
      form: {
        code: '',
        name: '',
        credits: '',
        grade: ''
      },
      showForm: false,
      isEditing: false,
      latestCourse: null,  // ตัวแปรใหม่สำหรับเก็บข้อมูลล่าสุด
    };
  },
  methods: {
    openForm(course = null) {
      if (course) {
        if (this.latestCourse && this.latestCourse.id !== course.id) {
          alert("ไม่สามารถแก้ไขข้อมูลนี้ได้");
          return;
        }
        this.form = { ...course };
        this.isEditing = true;
      } else {
        this.form = { code: '', name: '', credits: '', grade: '' };
        this.isEditing = false;
      }
      this.showForm = true;
    },
    saveCourse() {
      if (this.isEditing) {
        // ปรับปรุงข้อมูลใน courses
        const index = this.courses.findIndex(course => course.id === this.form.id);
        if (index !== -1) {
          this.courses[index] = { ...this.form };
        }
      } else {
        // เพิ่มข้อมูลใหม่
        const newCourse = { ...this.form, id: Date.now() }; // ใช้เวลาปัจจุบันเพื่อเป็น ID
        this.courses.push(newCourse);
        this.latestCourse = newCourse; // อัพเดตข้อมูลล่าสุด
      }
      this.closeForm();
    },
    deleteCourse(id) {
      this.courses = this.courses.filter(course => course.id !== id);
      if (this.latestCourse && this.latestCourse.id === id) {
        this.latestCourse = null; // ถ้าลบข้อมูลล่าสุด ต้องรีเซ็ตข้อมูลล่าสุด
      }
    },
    closeForm() {
      this.showForm = false;
      this.isEditing = false;
    }
  }
};
</script>


<style>
.course-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
  padding: 10px;
}

.course-card {
  background: #e2dadd; /* สีพื้นหลัง */
  border-left: 5px solid #395082; /* แถบสีด้านข้าง */
  padding: 20px;
  border-radius: 12px;
  box-shadow: 4px 4px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease-in-out;
}

.course-card:hover {
  transform: scale(1.05);
}

h3 {
  color: #201e27;
  margin-bottom: 10px;
}

p {
  color: #395082;
  font-weight: 600;
}

.actions {
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
}

button {
  padding: 10px 15px;
  border: none;
  cursor: pointer;
  border-radius: 6px;
  font-weight: bold;
  transition: background 0.3s ease-in-out, transform 0.2s;
}

button:hover {
  opacity: 0.9;
  transform: scale(1.05);
}

button:nth-child(1) {
  background: #7ccaf2;
  color: #131e58;
}

button:nth-child(2) {
  background: #cc995f;
  color: #201e27;
}

.form-modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 25px;
  box-shadow: 0px 5px 20px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
  max-width: 400px;
  width: 90%;
  text-align: center;
}

.form-modal h2 {
  color: #395082;
  margin-bottom: 15px;
}

.form-modal input {
  width: 100%;
  padding: 8px;
  margin: 8px 0;
  border: 2px solid #8398bf;
  border-radius: 6px;
  font-size: 16px;
}

.form-modal button {
  margin-top: 10px;
  width: 48%;
}

.form-modal button:nth-child(1) {
  background: #4174be;
  color: white;
}

.form-modal button:nth-child(2) {
  background: #83708a;
  color: white;
}

</style>