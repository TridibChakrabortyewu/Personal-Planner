<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @delete="onDelete" @complete="onComplete"/>
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
    };
  },
  mounted() {
    fetch("https://my-json-server.typicode.com/iamsabbirsobhani/json-server-typicode/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch(new Error("Fetch error..."));
    // axios.get('http://jsonplaceholder.typicode.com/comments')
    //   .then((res) => (this.projects = res))
    //   .catch(new Error("Fetch error..."));
    //   console.log('axios')
  },
  methods: {
    onDelete(d) {
      this.projects = this.projects.filter((project) => {
        return project.id !== d
      });
    },
      onComplete(id){
      let p = this.projects.find((project) => {
        return project.id === id
      })
      p.complete = !p.complete
      // console.log(p)
      // console.log(this.projects[0].title)
    }
  },
};
</script>
