<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>{{ message }}</h1>
    <ul v-if="jobs.length">
      <li v-for="job in jobs" :key="job.id">
        <Job v-bind:job="job"/>
      </li>
    </ul>
  </div>
</template>

<script>
import Job from './components/Job.vue'

export default {
  name: 'app',
  components: {
    Job
  },
  data() {
    return {
      message: 'Builds',
      jobs: [
        {
          target: 'http://www.harounatraore.com',
          title: 'todo list',
          branch: 'master',
          status: 'succeed',
          lastBuild: '#21',
          commit: '34563ef',
          duration: '1 m 45 sec',
          finished: '10 days ago'
        },
        {
          target: 'http://www.harounatraore.com',
          title: 'cuid-cli',
          branch: 'master',
          status: 'failed',
          lastBuild: '#4',
          commit: '95374ac',
          duration: '0 m 37 sec',
          finished: '7 minutes ago'
        }
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
</style>
