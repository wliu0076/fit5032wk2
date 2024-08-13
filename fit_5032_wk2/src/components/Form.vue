<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <h1 class="text-center">User Information Form</h1>
<form @submit.prevent="submitForm">
  <div class="row mb-3">
    <div class="col-md-6 col-sm-6">
  <label for="username" class="form-label">Username</label>
  <input type="text" class="form-control" id="username"
         v-model="formData.username"
         @blur="() => validateName(true)"
         @input="() => validateName(false)" />
  <div v-if="errors.username" class="text-danger">{{ errors.username }}</div>
</div>

<div class="col-md-6 col-sm-6">
  <label for="password" class="form-label">Password</label>
  <input type="password" class="form-control" id="password"
         @blur="() => validatePassword(true)"
         @input="() => validatePassword(false)"
         v-model="formData.password" />
  <div v-if="errors.password" class="text-danger">{{ errors.password }}</div>
</div>
  </div>
  <div class="row mb-3">
    <div class="col-md-6 col-sm-6">
      <div class="form-check">
        <input type="checkbox" class="form-check-input" id="isAustralian" v-model="formData.isAustralian" />
        <label class="form-check-label" for="isAustralian">Australian Resident?</label>
      </div>
    </div>
    <div class="col-md-6 col-sm-6">
      <label for="gender" class="form-label">Gender</label>
      <select class="form-select" id="gender" v-model="formData.gender">
        <option value="male">Male</option>
        <option value="female">Female</option>
        <option value="other">Other</option>
      </select>
    </div>
  </div>
  <div class="mb-3">
    <label for="reason" class="form-label">Reason for joining</label>
    <textarea class="form-control" id="reason" rows="3" v-model="formData.reason"></textarea>
  </div>
  <div class="text-center">
    <button type="submit" class="btn btn-primary me-2">Submit</button>
    <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
  </div>
</form>

        <div class="row mt-5" v-if="submittedCards.length">
          <div class="d-flex flex-wrap justify-content-start">
            <div v-for="(card, index) in submittedCards" :key="index" class="card m-2" style="width: 18rem;">
              <div class="card-header">
                User Information
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item">Username: {{ card.username }}</li>
                <li class="list-group-item">Password: {{ card.password }}</li>
                <li class="list-group-item">Australian Resident: {{ card.isAustralian ? 'Yes' : 'No' }}</li>
                <li class="list-group-item">Gender: {{ card.gender }}</li>
                <li class="list-group-item">Reason: {{ card.reason }}</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>





<script setup>
import { ref } from 'vue';
  
  const formData = ref({
      username: '',
      password: '',
      isAustralian: false,
      reason: '',
      gender: ''
  });
  
  const submittedCards = ref([]);
  
  const submitForm = () => {
  validateName(true);  // 调用validateName函数进行用户名验证，传递true表示检查失焦事件
  if (!errors.value.username) {  // 如果没有错误信息（即用户名验证通过）
    submittedCards.value.push({ ...formData.value });  // 将当前表单数据添加到submittedCards数组中
    clearForm();  // 调用clearForm函数清空表单
  }
};





  const clearForm = () => {
    for (const key in formData.value) {
        if (typeof formData.value[key] === 'boolean') {
            formData.value[key] = false;
        } else {
            formData.value[key] = '';
        }
    }
};
const errors = ref({
  username: null,
  password: null,
  resident: null,
  gender: null,
  reason: null
});
const validateName = (blur) => {
  if (formData.value.username.length < 3) {
    if (blur) errors.value.username = "Name must be at least 3 characters";
  } else {
    errors.value.username = null;
  }
};
const validatePassword = (blur) => {
  const password = formData.value.password;
  const minLength = 8;
  const hasUppercase = /[A-Z]/.test(password);
  const hasLowercase = /[a-z]/.test(password);
  const hasNumber = /\d/.test(password);
  const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(password);

  if (password.length < minLength) {
    if (blur) errors.value.password = `Password must be at least ${minLength} characters long.`;
  } else if (!hasUppercase) {
    if (blur) errors.value.password = "Password must contain at least one uppercase letter.";
  } else if (!hasLowercase) {
    if (blur) errors.value.password = "Password must contain at least one lowercase letter.";
  } else if (!hasNumber) {
    if (blur) errors.value.password = "Password must contain at least one number.";
  } else if (!hasSpecialChar) {
    if (blur) errors.value.password = "Password must contain at least one special character.";
  } else {
    errors.value.password = null;
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
