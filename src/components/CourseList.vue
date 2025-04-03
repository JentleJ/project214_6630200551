<template>
    <div class="container" sty>
      <h2>รายละเอียดการเรียน</h2>
      <table class="custom-table">
        <thead>
          <tr>
            <th>รหัสวิชา</th>
            <th>ชื่อวิชา</th>
            <th>หน่วยกิต</th>
            <th>เกรด</th>
            <th>จัดการ</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="course in courses" :key="course.id">
            <td>{{ course.code }}</td>
            <td>{{ course.name }}</td>
            <td>{{ course.credits }}</td>
            <td>{{ course.grade }}</td>
            <td>
              <button class="btn btn-warning btn-sm" @click="editCourse(course)">แก้ไข</button>
              <button class="btn btn-danger btn-sm" @click="deleteCourse(course.id)">ลบ</button>
            </td>
          </tr>
        </tbody>
      </table>
  
      <h3>{{ editMode ? "แก้ไขวิชา" : "เพิ่มวิชาใหม่" }}</h3>
      <form @submit.prevent="saveCourse">
        <input v-model="courseData.code" placeholder="รหัสวิชา" required />
        <input v-model="courseData.name" placeholder="ชื่อวิชา" required />
        <input v-model="courseData.credits" type="number" placeholder="หน่วยกิต" required />
        <input v-model="courseData.grade" placeholder="เกรด" required />
        <button type="submit" class="btn btn-success">{{ editMode ? "บันทึกการแก้ไข" : "เพิ่มวิชา" }}</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        courses: [],
        courseData: { code: "", name: "", credits: "", grade: "" },
        editMode: false,
        editId: null
      };
    },
    async mounted() {
      let response = await fetch("http://localhost:3000/courses");
      this.courses = await response.json();
    },
    methods: {
      async saveCourse() {
        if (this.editMode) {
          await fetch(`http://localhost:3000/courses/${this.editId}`, {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(this.courseData)
          });
        } else {
          await fetch("http://localhost:3000/courses", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(this.courseData)
          });
        }
        this.reloadData();
        this.resetForm();
      },
      async deleteCourse(id) {
        await fetch(`http://localhost:3000/courses/${id}`, { method: "DELETE" });
        this.reloadData();
      },
      editCourse(course) {
        this.editMode = true;
        this.editId = course.id;
        this.courseData = { ...course };
      },
      async reloadData() {
        let response = await fetch("http://localhost:3000/courses");
        this.courses = await response.json();
      },
      resetForm() {
        this.editMode = false;
        this.courseData = { code: "", name: "", credits: "", grade: "" };
      }
    }
  };
</script>

<style scoped>

.container {
    padding-bottom: 60px; /* Add padding to prevent sticking */
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    
}
.custom-table {
  width: 100%;
  border-collapse: collapse;
  background-color: black; /* Black background */
  color: white; /* White text */
  border: 1px solid #444;
  margin-bottom: 30px;
}

.custom-table th,
.custom-table td {
  padding: 10px;
  text-align: left;
  border: 1px solid #444;
}

.custom-table th {
  background-color: #222; /* Slightly lighter black for header */
  color: #ffcc00; /* Highlight header text */
}

.custom-table tr:nth-child(even) {
  background-color: #333; /* Alternate row color */
}

.custom-table tr:hover {
  background-color: #555; /* Highlight row on hover */
}
</style>

