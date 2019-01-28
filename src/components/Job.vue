<template>
  <table class="job">
    <tr>
      <th>
        <a :href="job.target" :title="job.title" :class="job.status" target="_blank">
          {{ job.title }}
        </a>
      </th>
      <td>
        {{ job.branch }}
      </td>
      <td>
        {{ job | formatStatus }}
      </td>
      <td>
        {{ job.commit }}
      </td>
      <td>
        {{ job.duration | formatDuration }}
      </td>
      <td>
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
    formatStatus: job => `${job.lastBuild} ${job.status}`,
    formatDuration: time => '',
    formatRelativeDate: time => {
      const when = Date.parse(time)
      const diff = Date.now() - when
      [
        {label: 'seconds', value: 1000},
        {label: 'minutes', value: 60},
        {label: 'hours', value: 60},
        {label: 'days', value: 24},
        {label: 'months', value: 30.42},
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



      const seconds = Math.floor(diff / 1000)
      const minutes = Math.floor(diff / (60 * 1000))
      const hours = Math.floor(diff / (60 * 60 * 1000))
      const days = Math.floor(diff / (24 * 60 * 60 * 1000))
      const months = Math.floor(diff / (30 * 24 * 60 * 60 * 1000))
      const suffix = seconds / 1000 < 60
        ? 'seconds ago'
        : seconds / (60 * 1000)
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
a {
  color: #42b983;
}

.pending {}
.succeed {
  background-color: green;
}
.failed {
  background-color: red;
}
.disabled {
  background-color: grey;
}
</style>
