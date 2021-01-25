<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="onDelete"
          @complete="onComplete"
        />
      </div>
    </div>
    <div v-else>
      <p>Project loading...</p>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";

export default {
  name: "Home",
  components: { SingleProject },
  data() {
    return {
      projects: [],
      // url: "http://localhost:3000/projects",
      url:
        "https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects",
    };
  },
  mounted() {
    fetch(this.url)
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch(new Error("Fetch error..."));
  },
  methods: {
    onDelete(d) {
      this.projects = this.projects.filter((project) => {
        return project.id !== d;
      });
    },
    onComplete(id) {
      let p = this.projects.find((project) => {
        return project.id === id;
      });
      p.complete = !p.complete;
    },
  },
};
</script>
