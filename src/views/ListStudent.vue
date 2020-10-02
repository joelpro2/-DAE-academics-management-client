<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-data-table
          :headers="fields"
          :items="students"
          :items-per-page="5"
          class="elevation-1">
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',

  created () {
    this.axios.get('/students/all')
      .then((response) => {
        this.students = response.data
      })
  },
  data: () => ({
    fields: [
      { text: 'Username', value: 'username' },
      { text: 'Name', value: 'name' },
      { text: 'Email', value: 'email' },
      { text: 'Course', value: 'courseName' }
    ],
    students: []
  })
}
</script>
