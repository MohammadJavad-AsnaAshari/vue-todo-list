<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 class="no-select" @click="showDetails = !showDetails">{{ project.title }}</h3>
      <div class="icons">
        <span @click="deleteProject" class="material-symbols-outlined no-select">delete</span>
        <router-link :to="{name: 'EditProject', params: {id: project.id}}">
          <span class="material-symbols-outlined no-select">edit</span>
        </router-link>
        <span @click="changeComplete" class="material-symbols-outlined no-select">check</span>
      </div>
    </div>
    <div v-if="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: [
    'project'
  ],
  data() {
    return {
      showDetails: false,
      uri: "http://localhost:3000/projects/" + this.project.id
    }
  },
  methods: {
    deleteProject() {
      fetch(this.uri, {method: 'DELETE'})
          .then(() => this.$emit('delete', this.project.id))
          .catch(err => console.log(err.message))
    },
    changeComplete() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({complete: !this.project.complete})
      })
          .then(() => this.$emit('complete', this.project.id))
          .catch(err => console.log(err.message))
    }
  }
}
</script>

<style scoped>

.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
  border-left: 4px solid #e90074;
}

h3 {
  cursor: pointer;
}

.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.material-symbols-outlined {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}

.material-symbols-outlined:hover {
  color: #777;
}

.project.complete {
  border-left: 4px solid #00ce89;
}

.no-select {
  user-select: none; /* Standard syntax */
  -webkit-user-select: none; /* Safari */
  -ms-user-select: none; /* IE 10+ */
}

</style>