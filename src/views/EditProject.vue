<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input v-model="title" type="text" required>
    <label>Details:</label>
    <textarea v-model="details"></textarea>
    <button>Edit Project</button>
  </form>
</template>

<script>
import router from "@/router";

export default {
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      uri: 'http://localhost:3000/projects/' + this.id
    }
  },
  mounted() {
    fetch(this.uri)
        .then(res => res.json())
        .then(data => {
          this.title = data.title
          this.details = data.details
        }).catch(err => console.log(err.message))
  },
  methods: {
    handleSubmit() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({
          title: this.title,
          details: this.details
        })
      }).then(() => router.push({name: 'home'}))
          .catch(err => console.log(err.message))
    }
  }
}
</script>

<style>
form {
  background: white;
  padding: 20px;
  border-radius: 10px;
}

label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  margin: 20px 0 10px 0;
}

input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}

textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
}

form button {
  display: block;
  margin: 20px auto 0;
  background: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
}
</style>