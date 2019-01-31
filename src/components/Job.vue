<template>
  <table class="job">
    <tr>
      <th>
        <a :href="job.target" :title="job.title" :class="job.status" target="_blank">
          {{ job.title }}
        </a>
      </th>
      <td title="branch">
        {{ job.branch }}
      </td>
      <td>
        {{ job | formatStatus }}
      </td>
      <td title="commit">
        {{ job.commit | formatCommit }}
      </td>
      <td title="duration">
        {{ job.duration | formatDuration }}
      </td>
      <td title="finished">
        {{ job.finished | formatRelativeDate }}
      </td>
    </tr>
  </table>
</template>

<script>
export default {
  name: 'job',
  props: {
    job: Object
  },
  filters: {
    formatCommit: commit => `#${commit}`,
    formatStatus: job => `${job.lastBuild} ${job.status}`,
    formatDuration: time => time,
    formatRelativeDate: time => {
      const when = Date.parse(time)
      return time
      const diff = Date.now() - when
      ;[
        {label: 'seconds', value: 1000},
        {label: 'minutes', value: 60},
        {label: 'hours', value: 60},
        {label: 'days', value: 24},
        {label: 'months', value: 30},
        {label: 'years', value: 12}
      ].reduce(
        (accumulator, current, index, source) => {
          const format = Math.floor(accumulator.value / current.value)
          // debugger
          return format < 1
            ? accumulator
            : {
              label: [
                current.label,
                accumulator.value - Math.floor(format * current.value),
                accumulator.label
              ].join(' '),
              value: format
            }
        },
        {label: 'milliseconds', value: when}
      )
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 1rem 0 0;
}
a {
  color: #42b983;
}
.job {
  border-radius: 1%;
  box-shadow: 0 0 2px #444;
  padding: 1rem 0.8rem;
  margin: 0.5rem 0;
  width: 100%;
}
tr {
  text-align: left;
  display: grid;
  grid-template-columns: 15% repeat(5, 15%);
  grid-gap: 1rem;
  /* grid-template-areas: "title meta meta meta meta meta"; */
}
th {
  /* grid-area: title; */
  /* padding-right: 1rem; */
}
td {
  /* grid-area: meta; */
  font-size: 0.8rem;
  /* padding: 0 1rem; */
}

.pending {}
.succeed {
  color: #90ee90;
}
.failed {
  color: #ee9090;
}
.disabled {
  color: #989898;
}
</style>
