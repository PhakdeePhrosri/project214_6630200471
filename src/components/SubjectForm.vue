<template>
  <div class="modal">
    <div class="modal-content">
      <h2>{{ isEditing ? "Edit Subject" : "Add Subject" }}</h2>
      
      <label>Subject Code:</label>
      <input v-model="subject.id" type="text" :disabled="isEditing" />
      
      <label>Subject Name:</label>
      <input v-model="subject.name" type="text" />
      
      <label>Credits:</label>
      <input v-model.number="subject.credit" type="number" min="0" />
      
      <label>Grade:</label>
      <input v-model="subject.grade" type="text" />
      
      <div class="button-group">
        <button class="save-button" @click="saveSubject">Save</button>
        <button class="cancel-button" @click="$emit('cancel')">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    subjectData: Object,
    isEditing: Boolean
  },
  data() {
    return {
      subject: { ...this.subjectData }
    };
  },
  watch: {
    subjectData: {
      handler(newValue) {
        this.subject = { ...newValue };
      },
      immediate: true
    }
  },
  methods: {
    async saveSubject() {
  try {
    await fetch("http://localhost:3000/subjects", {
      method: this.isEditing ? "PUT" : "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(this.subject)
    });
    this.$emit("subject-added"); 
    this.$nextTick(() => {
      this.$emit("fetchSubjects"); // ดึงข้อมูลใหม่
    });
  } catch (error) {
    console.error("Error adding subject:", error);
  }
}
  }
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  width: 90%;
  max-width: 400px;
  z-index: 1000;
}

.modal-content {
  text-align: center;
}

.button-group {
  margin-top: 20px;
}

.save-button, .cancel-button {
  padding: 10px 15px;
  margin: 5px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.save-button {
  background: #0a2e56;
  color: white;
}

.cancel-button {
  background: #c0392b;
  color: white;
}
</style>