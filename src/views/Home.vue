<template>
  <div class="home">
    <FilterNav :current="current" @filterChange="current = $event"/>
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
import FilterNav from "../components/FilterNav.vue";

export default {
  name: "Home",
  components: { SingleProject, FilterNav },
  data() {
    return {
      i: 0,
      current: 'all',
      projects: [],
      url: "http://localhost:3000/projects",
      // url:
      //   "https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects",
    };
  },
  mounted() {
    fetch(this.url)
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch(new Error("Fetch error..."));
  },
  updated(){
    if(this.current === 'completed'){
      this.projects = this.projects.filter((project) => {
        return project.complete === true
      })
    }else if(this.current === 'all'){
      fetch(this.url)
      .then((res) => res.json())
      .then((data) => (this.projects = data) )
      .catch(new Error("Fetch error..."));

    }else if(this.current === 'ongoing'){
      this.projects = this.projects.filter((project) => {
        return project.complete !== true
      })
    }
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
