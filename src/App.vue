<template>
  <StudentProfile />
  <CourseList />
  <img :src="profileImage" alt="Profile Image" class="profile-image" />
</template>

<script>
import StudentProfile from './components/StudentProfile.vue';
import CourseList from './components/CourseList.vue';

export default {
  components: { StudentProfile, CourseList},
  data() {
    return {
      profileImage: require('@/assets/ME.png') 
    };
  },
};
</script>

<style scoped>
  .profile-image {
    display: block;
    margin: 0 auto;
    margin-top: 5px;
    margin-bottom: 25px;
    padding-bottom: 30px;
    width: 500px;
    height: auto;
    border-radius: 3ch;
  }
</style>