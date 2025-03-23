<template>
  <div class="animated-background">
    <nav class="navbar">
      <!-- ปุ่มเปิด/ปิดเสียง -->
      <button class="audio-control" :class="{ 'audio-on': isPlaying, 'audio-off': !isPlaying }" @click="toggleAudio">
        {{ isPlaying ? "🔊 ปิดเสียง" : "🔇 เปิดเสียง" }}
      </button>

      <!-- Slider ปรับระดับเสียง -->
      <input type="range" v-model="volume" min="0" max="1" step="0.1" @input="changeVolume" class="volume-slider" />
    </nav>

    <!-- แทร็กเสียง -->
    <audio ref="backgroundAudio" loop>
      <source :src="audioSrc" type="audio/wav" />
      Your browser does not support the audio element.
    </audio>

    <div class="main-container">
      <!-- Profile Card -->
      <div class="profile-container">
        <div class="card">
          <div class="image-container">
            <img :src="student.image" alt="Student Image" class="image-placeholder" />
            <button class="edit-button" @click="isEditing = true">Edit Profile</button>
          </div>
          <div class="text-section">
            <h1>{{ student.fullName }}</h1>
            <p><strong>Student ID:</strong> {{ student.id }}</p>
            <p><strong>Major:</strong> {{ student.major }}</p>
            <p><strong>Previous School:</strong> {{ student.previousSchool }}</p>
          </div>
        </div>
      </div>

      <!-- Edit Form -->
      <div v-if="isEditing" class="edit-form">
        <h3>Edit Profile</h3>
        <label>Full Name: <input v-model="student.fullName" type="text" /></label>
        <label>Student ID: <input v-model="student.id" type="text" /></label>
        <label>Major: <input v-model="student.major" type="text" /></label>
        <label>Previous School: <input v-model="student.previousSchool" type="text" /></label>
        <label>Profile Picture:
          <select v-model="student.image">
            <option v-for="img in availableImages" :key="img" :value="img">{{ img }}</option>
          </select>
        </label>
        <button class="save-button" @click="isEditing = false">Save</button>
        <button class="cancel-button" @click="isEditing = false">Cancel</button>
      </div>

      <!-- Subjects (Courses) -->
      <div class="course-container">
        <Course />
      </div>
    </div>
  </div>
</template>

<script>
import Course from './components/Course.vue';

export default {
  components: {
    Course
  },
  data() {
    return {
      student: {
        image: "./src/Media/1.jpg", // ใช้ path ตรงจาก public
        fullName: "Phakdee Phrosri",
        id: "6630200471",
        major: "Computer Science",
        previousSchool: "Nawamintrachinutit Suankulard Wittatalai Samutparkan School"
      },
      availableImages: [
        "./src/Media/1.jpg",
        "./src/Media/2.jpg"
      ],
      audioSrc: "./src/Media/New Dawn.wav", // ใช้ไฟล์จาก public
      isPlaying: false,
      isEditing: false,
      volume: 1, // ระดับเสียงเริ่มต้น 100%
    };
  },mounted() {
  this.isPlaying = false; // เริ่มต้นเป็นปิดเสียง
},
methods: {
    toggleAudio() {
      const audio = this.$refs.backgroundAudio;
      if (this.isPlaying) {
        audio.pause();
      } else {
        audio.play();
      }
      this.isPlaying = !this.isPlaying;
    },
    changeVolume() {
      this.$refs.backgroundAudio.volume = this.volume;
    }
  }
}
</script>

<style>
html, body {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}

.animated-background {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(45deg, #6e95df, #8398bf, #4174be, #7ccaf2, #395082);
  animation: gradientAnimation 15s ease infinite;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  margin: 0;
  overflow: hidden;
  position: fixed;
  top: 0;
  left: 0;
}

@keyframes gradientAnimation {
  0% {
    background: linear-gradient(45deg, #6e95df, #8398bf, #4174be, #7ccaf2, #395082);
  }
  25% {
    background: linear-gradient(60deg, #4174be, #7ccaf2, #6e95df, #8398bf, #395082);
  }
  50% {
    background: linear-gradient(90deg, #7ccaf2, #6e95df, #8398bf, #4174be, #395082);
  }
  75% {
    background: linear-gradient(120deg, #8398bf, #4174be, #6e95df, #7ccaf2, #395082);
  }
  100% {
    background: linear-gradient(45deg, #6e95df, #8398bf, #4174be, #7ccaf2, #395082);
  }
}





/* --- Navbar --- */
.navbar {
  position: fixed;
  top: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.5);
  padding: 10px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(10px);
  padding: 10px;
}

/* ปุ่มควบคุมเสียง */
.audio-control {
  padding: 8px 15px;
  border: none;
  cursor: pointer;
  font-size: 16px;
  color: white;
  border-radius: 5px;
}

.audio-control:hover {
  transform: scale(1.1);
}

.audio-on {
  background-color: #27ae60;
}

.audio-off {
  background-color: #c0392b;
}
/* Slider ปรับเสียง */
.volume-slider {
  width: 150px;
  margin-left: 10px;
}

/* --- Container หลัก --- */
.main-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;  /* จัดโปรไฟล์ไปทางซ้าย */
  align-items: center;  /* ทำให้โปรไฟล์อยู่ตรงกลางแนวตั้ง */
  width: 100%;
  height: 100vh; /* ให้เต็มหน้าจอ */
  padding: 20px;
  gap: 20px;
  overflow: hidden;
}

/* --- Profile Card --- */
.profile-container {
  flex: 1;
  min-width: 350px;
  max-width: 400px;
  margin-left: 5%; /* ปรับระยะห่างซ้าย */
  display: flex;
  justify-content: flex-start; /* ชิดซ้าย */
  margin-left: 50px; /* ระยะห่างจากขอบซ้าย */
  width: 40%;

}

.card {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  width: 100%;
  max-width: 350px;
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: scale(1.05);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
}

.image-placeholder {
  width: 100%;
  max-width: 220px;
  height: auto;
  border-radius: 50%;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  margin-bottom: 15px;
}

/* ปุ่ม Edit Profile */
.edit-button {
  margin-top: 10px;
  background: #3498db;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.2s;
}

.edit-button:hover {
  background: #2980b9;
  transform: scale(1.05);
}

/* --- Course Container --- */
.course-container {
  flex: 2;
  min-width: 500px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 15px;
  padding: 20px;
}

.course-card {
  background: white;
  border-radius: 10px;
  padding: 15px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  width: 300px;
  transition: all 0.3s ease;
  text-align: center;
}

.course-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
}

/* ปุ่ม Edit และ Delete */
button {
  background-color: #3498db;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.2s;
}

button:hover {
  background-color: #2980b9;
  transform: scale(1.05);
}

.delete-button {
  background-color: #e74c3c;
}

.delete-button:hover {
  background-color: #c0392b;
}
/* ส่วนของฟอร์มแก้ไข */
.edit-form {
  background: white;
  padding: 20px;
  margin-top: 10px;
  border-radius: 10px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}
.save-button {
  background-color: #2ecc71;
  color: white;
  padding: 8px 15px;
  border: none;
  cursor: pointer;
  margin-right: 5px;
}
.cancel-button {
  background-color: #e74c3c;
  color: white;
  padding: 8px 15px;
  border: none;
  cursor: pointer;
}
</style>
