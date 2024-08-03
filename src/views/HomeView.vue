<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from "@/components/SingleProject.vue";
import FilterNav from "@/components/FilterNavbar.vue";

export default {
  name: 'HomeView',
  components: {
    SingleProject,
    FilterNav
  },
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
        .then(res => res.json())
        .then(data => this.projects = data)
        .catch(err => console.log(err))
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter(item => {
        return item.id !== id
      })
    },
    handleComplete(id) {
      let project = this.projects.find(item => {
        return item.id === id
      })
      project.complete = !project.complete
    }
  },
  computed: {
    filteredProjects() {
      if (this.current === 'completed') {
        return this.projects.filter(item => item.complete)
      } else if (this.current === 'ongoing') {
        return this.projects.filter(item => !item.complete)
      } else {
        return this.projects
      }
    }
  }
}
</script>
