<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

defineProps<{ msg: string }>()

const apiResponse = ref({})

function query(){
  axios.get("http://localhost:8080/api/v1/info/hello").then(response => {
    apiResponse.value = response.data
  }).catch().finally()
}

function pdf(): void {
  let data = {"id": "some-id"};

  fetch("http://localhost:8080/api/v1/info/pdf", {
    "method": "POST",
    "headers": {'Content-Type': 'application/json'},
    "body": JSON.stringify(data)
  }).then(res => {
    console.log("Request complete! response:", res);
    res.blob().then(data => {
      var fileURL = URL.createObjectURL(data);
      window.open(fileURL);
    })


  });
}

</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="query()">Query API</button>
    <div v-if="Object.keys(apiResponse).length">API response was {{ apiResponse }}</div>
    <div v-else>

    </div>
  </div>

  <p>
    <button type="button" @click="pdf()">PDF</button>
  </p>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Install
    <a href="https://github.com/vuejs/language-tools" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
