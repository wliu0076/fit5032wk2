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
                @blur="() => validateName(true)"
                @input="() => validateName(false)"
                v-model="formData.username" />
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
                <input type="checkbox" class="form-check-input" id="isAustralian"
                  @blur="() => validateIsAustralian(true)"
                  @input="() => validateIsAustralian(false)" 
                  v-model="formData.isAustralian" />
                <label class="form-check-label" for="isAustralian">Australian Resident?</label>
              </div>
              <div v-if="errors.isAustralian" class="text-danger">{{ errors.isAustralian }}</div>
            </div>
          </div>
          <div class="row mb-3">
            <div class="col-md-6 col-sm-6">
              <label for="gender" class="form-label">Gender</label>
              <select class="form-select" id="gender"
                @blur="() => validateGender(true)"
                @change="() => validateGender(false)"
                v-model="formData.gender">
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="other">Other</option>
              </select>
              <div v-if="errors.gender" class="text-danger">{{ errors.gender }}</div>
            </div>
          </div>
          <div class="mb-3">
            <label for="reason" class="form-label">Reason for joining</label>
            <textarea class="form-control" id="reason" rows="3"
              @blur="() => validateReason(true)"
              @input="() => validateReason(false)"
              v-model="formData.reason"></textarea>
            <div v-if="errors.reason" class="text-danger">{{ errors.reason }}</div>
          </div>
          <div class="text-center">
            <button type="submit" class="btn btn-primary me-2">Submit</button>
            <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
          </div>
        </form>
        
         <!-- Display submitted cards -->
         <div class="row mt-5" v-if="submittedCards.length">
          <div class="d-flex flex-wrap justify-content-center">
            <DataTable :value="submittedCards" tableStyle="min-width: 50%">
              <Column field="username" header="Name"></Column>
              <Column field="password" header="Password"></Column>
              <Column field="isAustralian" header="Australian Resident"></Column>
              <Column field="gender" header="Gender"></Column>
              <Column field="reason" header="Reason"></Column>
            </DataTable>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';

const formData = ref({
  username: '',
  password: '',
  isAustralian: false,
  reason: '',
  gender: ''
});

const submittedCards = ref([]);

const submitForm = () => {
  validateName(true);
  validatePassword(true);
  validateIsAustralian(true);
  validateGender(true);
  validateReason(true);

  // Additional Validations
  validateUsernameCharacters(true); // #1: Ensure username only contains letters and numbers
  validatePasswordLength(true); // #2: Ensure password is not too long
  validateReasonSpecialChars(true); // #3: Ensure reason doesn't contain special characters
  validateGenderOption(true); // #4: Ensure gender selection is valid
  validateReasonCase(true); // #5: Ensure reason starts with a capital letter

  // Check all validation errors including additional validations
  if (
    !errors.value.username &&
    !errors.value.password &&
    !errors.value.isAustralian && // Corrected from errors.value.resident
    !errors.value.gender &&
    !errors.value.reason
  ) {
    submittedCards.value.push({
      ...formData.value
    });
    clearForm();
  }
};

const clearForm = () => {
  formData.value = {
    username: '',
    password: '',
    isAustralian: false,
    gender: '',
    reason: ''
  };
};

const errors = ref({
  username: null,
  password: null,
  isAustralian: null, // Corrected to match validation
  gender: null,
  reason: null,
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

const validateIsAustralian = (blur) => {
  if (formData.value.isAustralian === null || formData.value.isAustralian === undefined) { 
    if (blur) errors.value.isAustralian = "Please indicate if you are an Australian resident.";
  } else {
    errors.value.isAustralian = null; 
  }
};

const validateGender = (blur) => {
  if (!formData.value.gender) {
    if (blur) errors.value.gender = "Please select your gender.";
  } else {
    errors.value.gender = null;
  }
};

const validateReason = (blur) => {
  if (formData.value.reason.length < 10) {
    if (blur) errors.value.reason = "Reason must be at least 10 characters long.";
  } else {
    errors.value.reason = null;
  }
};

// #1: Validate that the username only contains alphanumeric characters
const validateUsernameCharacters = (blur) => {
  const usernameRegex = /^[a-zA-Z0-9]*$/;
  if (!usernameRegex.test(formData.value.username)) {
    if (blur) errors.value.username = "Username can only contain letters and numbers.";
  } else {
    errors.value.username = null;
  }
};

// #2: Validate that the password is not longer than 16 characters
const validatePasswordLength = (blur) => {
  if (formData.value.password.length > 16) {
    if (blur) errors.value.password = "Password cannot be longer than 16 characters.";
  } else {
    errors.value.password = null;
  }
};

// #3: Validate that the reason does not contain special characters
const validateReasonSpecialChars = (blur) => {
  const reasonRegex = /^[a-zA-Z0-9\s]*$/;
  if (!reasonRegex.test(formData.value.reason)) {
    if (blur) errors.value.reason = "Reason cannot contain special characters.";
  } else {
    errors.value.reason = null;
  }
};

// #4: Validate that the selected gender is within the provided options
const validateGenderOption = (blur) => {
  const validGenders = ['male', 'female', 'other'];
  if (!validGenders.includes(formData.value.gender)) {
    if (blur) errors.value.gender = "Please select a valid gender.";
  } else {
    errors.value.gender = null;
  }
};

// #5: Validate that the reason starts with a capital letter
const validateReasonCase = (blur) => {
  if (formData.value.reason.charAt(0) !== formData.value.reason.charAt(0).toUpperCase()) {
    if (blur) errors.value.reason = "Reason must start with a capital letter.";
  } else {
    errors.value.reason = null;
  }
};

</script>
  
<style scoped>
.card {
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.card-header {
  background-color: #275FDA;
  color: white;
  padding: 10px;
  border-radius: 10px 10px 0 0;
}
.list-group-item {
  padding: 10px;
}
</style>
