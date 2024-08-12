<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-12 col-sm-10 offset-sm-1 col-md-8 offset-md-2 col-lg-6 offset-lg-3">
        <h1 class="text-center mb-4">User Information Form</h1>
        <form @submit.prevent="submitForm" class="needs-validation" novalidate>
          <div class="mb-3">
            <label for="username" class="form-label">Username</label>
            <input type="text" class="form-control" id="username" v-model="formData.username" required>
          </div>
          <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input type="password" class="form-control" id="password" v-model="formData.password" required>
          </div>
          <div class="mb-3 form-check">
            <input type="checkbox" class="form-check-input" id="isAustralian" v-model="formData.isAustralian">
            <label class="form-check-label" for="isAustralian">Australian Resident?</label>
          </div>
          <div class="mb-3">
            <label for="gender" class="form-label">Gender</label>
            <select class="form-select" id="gender" v-model="formData.gender">
              <option value="male">Male</option>
              <option value="female">Female</option>
              <option value="other">Other</option>
            </select>
          </div>
          <div class="mb-4">
            <label for="reason" class="form-label">Reason for joining</label>
            <textarea class="form-control" id="reason" rows="3" v-model="formData.reason" required></textarea>
          </div>
          <div class="d-grid gap-2 col-6 mx-auto">
            <button type="submit" class="btn btn-primary">Submit</button>
            <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>


<script setup>
import { ref } from 'vue';

const formData = reactive({
  username: '',
  password: '',
  isAustralian: false,
  gender: '',
  reason: ''
});

const submittedCards = ref([]);

const submitForm = () => {
  submittedCards.value.push({
      ...formData.value
  });
  clearForm();
};

import { reactive } from 'vue';



const clearForm = () => {
  for (const key in formData) {
      if (typeof formData[key] === 'boolean') {
          formData[key] = false;
      } else {
          formData[key] = '';
      }
  }
};
</script>

<style scoped>
@media (max-width: 576px) {
.form-control, .form-select {
  font-size: 14px; /* Smaller font size */
}
.btn {
  padding: 10px 15px; /* Button size suitable for touch screens */
}
}

@media (min-width: 577px) and (max-width: 768px) {
.form-control, .form-select {
  font-size: 16px; /* Slightly larger font size */
}
.btn {
  padding: 12px 20px; /* Button size for larger screens */
}
}

@media (min-width: 769px) {
.form-control, .form-select {
  font-size: 18px; /* Font size suitable for desktop displays */
}
.btn {
  padding: 14px 24px; /* Larger button size */
}
}
</style>
