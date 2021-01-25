<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <!-- {{ diffActions }} -->
      <div v-for="project in filteredProjects" :key="project.id">
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
import FilterNav from "../components/FilterNav.vue";

export default {
  name: "Home",
  components: { SingleProject, FilterNav },
  data() {
    return {
      current: "all",
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
    onFetch() {
      fetch(this.url)
        .then((res) => res.json())
        .then((data) => (this.projects = data))
        .catch(new Error("Fetch error..."));
    },
    onComplete(id) {
      //onComplete(id) is for updating local data
      //only one single object will be given into "p"
      //according to the completed id
      let p = this.projects.find((project) => {
        return project.id === id;
      });
      p.complete = !p.complete;
    },
  },
  computed: {
    filteredProjects() {
      if (this.current === "completed") {
        return this.projects.filter((project) => project.complete);
      }
      if (this.current === "ongoing") {
        return this.projects.filter((project) => !project.complete);
      }
      return this.projects;
    },
  },
};
</script>
