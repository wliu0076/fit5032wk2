<!-- eslint-disable vue/return-in-computed-property -->
<!-- JSONLab.vue -->
<template>
  <div class="json-lab">
    <h1>🗄️ JSON Data & Vue Directives Lab</h1>

    <section class="lab-section">
      <h2>📚 Working with JSON Arrays</h2>
      <p>Our <code>authors.json</code> contains an array of author objects.</p>
      
      <h3 class="activity-title">1. Iterating through Arrays:</h3>
      <!-- Activity 6: Render a list containing author names and their birth years. -->
      <div class="author-list">
        <div v-for="author in authors" :key="author.id">
          <span class="author-info">{{ author.name }} ({{ author.birthYear }})</span>
        </div>
      </div>

      <h3 class="activity-title">2. Filtering Arrays</h3>
      <!-- Activity 7: Render a list containing authors born after 1850. -->
      <div class="author-list">
        <div v-for="author in authors.filter(a => a.birthYear > 1850)" :key="author.id">
          <span class="author-info">{{ author.name }} ({{ author.birthYear }})</span>
        </div>
      </div>

      <h3 class="activity-title">3. Mapping Arrays</h3>
      <p>Famous works:</p>
      <!-- Activity 8: Render a list of all famous works. -->
      <ul class="works-list">
        <li v-for="work in authors.flatMap(author => author.famousWorks)" :key="work.id">
          <span class="work-info">{{ work.title }}</span>
        </li>
      </ul>

      <h3 class="activity-title">4. Finding in Arrays</h3>
      <p class="author-info">Finding by property: {{ orwell?.name }}</p>
      
      <h3 class="activity-title">5. Nested Arrays/Objects</h3>
      <p>{{ austen?.name }}'s works:</p>
      <!-- Activity 9: Render a list of Austen's works. -->
      <div class="works-list">
        <div v-for="work in authors.find(author => author.id === 1).famousWorks" :key="work.id">
          <span class="work-info">{{ work.title }}</span>
        </div>
      </div>
    </section>

<section class="lab-section">
      <h2>🏢 Working with JSON Objects</h2>
      <p>Our <code>bookstores.json</code> is a JSON object.</p>

      <h3 class="activity-title">Accessing Properties</h3>
      <p>
        Company:
        <span class="answer">Worldwide Bookstores</span>
      </p>

      <p>
        Total Stores:
        <span class="answer">5</span>
      </p>

      <h3 class="activity-title">Iterating Object Properties</h3>
      <p>Store Types:</p>
      <ul>
        <li v-for="(count, type) in bookstores.storeTypes" :key="type">
          <span class="answer">{{ type }}: {{ count }} store(s)</span>
        </li>
      </ul>

      <h3 class="activity-title">Nested Objects</h3>
      <p>Opening Hours:</p>
      <ul>
        <li v-for="(hours, day) in bookstores.openingHours" :key="day">
          <span class="answer">{{ day }}: Open from {{ hours.open }} to {{ hours.close }}</span>
        </li>
      </ul>

      <h3 class="activity-title">Working with Arrays in Objects</h3>
<p>We operate in:</p>
<ul>
  <li v-for="country in bookstores.countries" :key="country">
    <span class="answer">{{ country }}</span>
  </li>
</ul>
<p><span class="answer">{{ bookstores.topSeller }}</span></p>

      <p>Our #1 seller:</p>
      <p><span class="answer">To Kill a Mockingbird</span></p>
    </section>

    <section class="lab-section">
      <h2>v-if & v-else</h2>
      <p>Toggle visibility based on a condition.</p>
      <button @click="showMessage = !showMessage">Toggle Message</button>
      <p v-if="showMessage" class="message success"><span class="answer">✨ You're a Vue superstar! ✨</span></p>
    </section>

    <section class="lab-section">
  <h2>Attribute, Class and Style Binding with <code>v-bind</code></h2>
  <p>Highlighting Specific Authors:</p>
  <ul>
    <li v-for="author in authors" :key="author.id"
        :style="{
          color: author.name === 'George Orwell' ? 'blue' : 'black',
          fontWeight: author.name === 'George Orwell' ? 'bold' : 'normal',
          fontSize: author.name === 'George Orwell' ? '1.2em' : '1em'
        }">
      ID: {{ author.id }}, Name: {{ author.name }}
    </li>
  </ul>
</section>

    <!-- <p>{{modernAuthors}}</p> -->
  </div>
</template>

<script setup>
import { ref, computed } from "vue"

// Activity 1: Import JSON files (authors.json and bookstores.json)
// TODO: CODE TO IMPORT JSON FILES HERE
import authors from "../assets/json/authors.json"
import bookstores from "../assets/json/bookstores.json"

const showMessage = ref(false)

// Activity 2: Get authors born after 1850
const modernAuthors = computed(() => {
  // TODO: CODE TO FILTER ARRAY OF AUTHORS HERE
  return authors.filter((author) => author.birthYear > 1850)
})

// Activity 3: Get all famous works
const allFamousWorks = computed(() => {
  return authors.flatMap((author) => author.famousWorks.map((work) => work.title))
})

// Activity 4: Find author by name
const orwell = computed(() => {
  return authors.find(author => author.name === "George Orwell")
})

// Activity 5: Find author by ID
const austen = computed(() => {
  return authors.find(author => author.id ==1)})
</script>

<style scoped>
.json-lab {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  max-width: 80vw;
  margin: 0 auto;
  padding: 20px;
  background-color: #f4f4f4;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1,
h2 {
  color: #333;
}
h1 {
  text-align: center;
}

.lab-section {
  background-color: white;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.message {
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
}

.success {
  background-color: #e7faf3;
  color: #42b883;
  border: 1px solid #42b883;
}

.highlight {
  background-color: #42b883;
}

code {
  background-color: #e0e0e0;
  padding: 2px 5px;
  border-radius: 4px;
  font-family: "Courier New", Courier, monospace;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  background-color: #f0f0f0;
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
}
.json-lab .activity-title {
  color: red;
}

.json-lab .answer {
  color: blue;
}

.message.success {
  color: green; /* Example of additional styling */
}

</style>
