<template>
  <div class="container text-center">
    <h2>ข้อมูลนิสิต</h2>
    <img :src="profileImage" alt="Profile Image" class="profile-image" />
    <p><strong>ชื่อ:</strong> {{ student.name }}</p>
    <p><strong>รหัสนิสิต:</strong> {{ student.student_id }}</p>
    <p><strong>สาขา:</strong> {{ student.major }}</p>
    <p><strong>โรงเรียนเดิม:</strong> {{ student.school }}</p>
    
    <button class="btn btn-outline-secondary mb-3" @click="editMode = true">แก้ไขข้อมูล</button>
    
    <form v-if="editMode" @submit.prevent="saveData">
      <input v-model="student.name" placeholder="ชื่อ" required />
      <input v-model="student.student_id" placeholder="รหัสนิสิต" required />
      <input v-model="student.major" placeholder="สาขา" required />
      <input v-model="student.school" placeholder="โรงเรียนเดิม" required />
      <button type="submit" class="btn btn-success mt-3">บันทึก</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      student: {},
      editMode: false,
      profileImage: require('@/assets/ME1.png') // Update the path to your new profile image
    };
  },
  
  async mounted() {
    let response = await fetch("http://localhost:3000/student");
    this.student = await response.json();
  },
  methods: {
    async saveData() {
      await fetch("http://localhost:3000/student", {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.student)
      });
      this.editMode = false;
    }
  }
};
</script>

<style scoped>
.profile-image {
  display: block;
  margin: 0 auto;
  margin-top: 25px;
  margin-bottom: 25px;
  width: 200px;
  height: auto;
  border-radius: 3ch;
}

.container {

    font-size: 16px; /* Set the font size */
  padding-bottom: 20px; /* Add padding to prevent sticking */
}
</style>
