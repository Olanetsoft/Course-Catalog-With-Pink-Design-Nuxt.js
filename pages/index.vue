<template>
  <div>
    <div class="container">
      <div class="sidebar">
        <h3>Categories</h3>
        <div class="categories">
          <div
            v-for="category in categories"
            :key="category.id"
            class="category"
            :class="{ active: selectedCategories.includes(category.id) }"
            @click="toggleCategory(category.id)"
          >
            {{ category.name }}
          </div>
        </div>
      </div>
      <div class="content">
        <h1 class="course-catalogue-title">
          Course Catalog With Pink Design and Nuxt.js
        </h1>
        <div class="search">
          <input
            type="text"
            placeholder="Search Courses..."
            v-model="searchQuery"
          />
        </div>
        <div class="courses">
          <div
            class="course-card"
            v-for="course in filteredCourses"
            :key="course.id"
          >
            <div class="card" @click="openCourseModal(course)">
              <div class="card-content">
                <h3 class="title">{{ course.title }}</h3>
                <div class="description">{{ course.description }}</div>
                <div class="details">
                  <div class="level">{{ course.level }}</div>
                </div>
              </div>
            
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="course-modal" v-if="selectedCourse">
      <div class="modal-content">
        <span class="close-modal" @click="closeCourseModal">&times;</span>
        <h2 class="modal-title">{{ selectedCourse.title }}</h2>
        <div class="modal-description">{{ selectedCourse.description }}</div>
        <div class="details">
          <div class="modal-level">{{ selectedCourse.level }}</div>
        </div>
        <div class="modal-prerequisites">
          <h3>Prerequisites</h3>
          <ul v-if="selectedCourse.prerequisites.length">
            <li
              v-for="prerequisite in selectedCourse.prerequisites"
              :key="prerequisite"
            >
              {{ prerequisite }}
            </li>
          </ul>
          <p v-else>None</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// Import courses data from JSON file
import coursesData from "../courses.json";
export default {
  data() {
    return {
      categories: [
        { id: 1, name: "Web Development" },
        { id: 2, name: "Mobile" },
        { id: 3, name: "Data Science" },
        { id: 4, name: "UI/UX Design" },
        { id: 5, name: "Cloud Computing" },
      ],
      courses: coursesData,
      selectedCategories: [],
      searchQuery: "",
      selectedCourse: null,
    };
  },
  methods: {
    toggleCategory(categoryId) {
      if (this.selectedCategories.includes(categoryId)) {
        this.selectedCategories = this.selectedCategories.filter(
          (id) => id !== categoryId
        );
      } else {
        this.selectedCategories.push(categoryId);
      }
    },
    openCourseModal(course) {
      this.selectedCourse = course;
    },
    closeCourseModal() {
      this.selectedCourse = null;
    },
  },
  computed: {
    filteredCourses() {
      return this.courses.filter((course) => {
        if (
          this.selectedCategories.length &&
          !this.selectedCategories.includes(course.category)
        ) {
          return false;
        }
        if (
          this.searchQuery &&
          !course.title.toLowerCase().includes(this.searchQuery.toLowerCase())
        ) {
          return false;
        }
        return true;
      });
    },
  },
};
</script>

<style>
@import "@/styles.css";
</style>
