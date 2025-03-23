<template>
    <div class="subject-card">
      <h3>{{ subject.name }}</h3>
      <p><strong>Credits:</strong> {{ subject.credit }}</p>
      <p><strong>Grade:</strong> {{ subject.grade }}</p>
  
      <button class="edit-button" @click="$emit('edit', subject)">Edit</button>
      <button class="delete-button" @click="$emit('delete', subject.id)">Delete</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      subject: Object
    }
  };
  </script>
  