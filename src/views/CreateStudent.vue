<template>
  <div class="create-student">
    <v-container>
      <v-row>
        <v-col cols="12">
          <v-btn @click="() => { $router.go(-1) }"
                 class="mt-2"
                 text>
            <v-icon left>navigate_before</v-icon>
            <span class="mr-2">Back</span>
          </v-btn>
        </v-col>
        <v-col cols="12"><h1>Create Student</h1></v-col>
      </v-row>
      <v-form @submit.prevent="create">
        <v-row>
          <v-col md="6" sm="12">
            <v-text-field label="Username"
                          type="text"
                          :error-messages="usernameErrorBag"
                          v-model="student.username"
            ></v-text-field>
          </v-col>
          <v-col md="6" sm="12">
            <v-text-field :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                          :type="showPassword ? 'text' : 'password'"
                          @click:append="showPassword = !showPassword"
                          label="Password"
                          :error-messages="passwordErrorBag"
                          v-model="student.password"
            ></v-text-field>
          </v-col>
          <v-col md="6" sm="12">
            <v-text-field label="Name"
                          type="text"
                          :error-messages="nameErrorBag"
                          v-model="student.name"
            ></v-text-field>
          </v-col>
          <v-col md="6" sm="12">
            <v-text-field label="Email"
                          type="email"
                          :error-messages="emailErrorBag"
                          v-model="student.email"
            ></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-select
              v-model="student.courseCode"
              :items="courses"
              item-text="name"
              item-value="code"
              label="Course"
              multiple
              chips
            ></v-select>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="auto">
            <v-btn
              @click="$v.student.$reset()"
              color="primary">RESET</v-btn>
          </v-col>
          <v-col cols="auto">
            <v-btn
              type="submit"
              color="primary">CREATE</v-btn>
          </v-col>
        </v-row>
      </v-form>
    </v-container>
  </div>
</template>

<script>
import { email, required } from 'vuelidate/lib/validators'

export default {
  name: 'CreateStudent',

  created () {},
  data: () => ({
    showPassword: false,
    courses: [
      { name: 'Engenharia Informática', code: 1 },
      { name: 'Engenharia e Gestão Industrial', code: 2 },
      { name: 'Nutrição e Dietética', code: 3 }],
    student: {}
  }),
  validations: {
    student: {
      username: {
        required
      },
      password: {
        required
      },
      name: {
        required
      },
      email: {
        required,
        email
      }
    }
  },
  computed: {
    isValid () {
      this.$v.student.$touch()
      return !this.$v.student.$invalid
    },
    // Error bags
    usernameErrorBag () {
      const errorBag = []
      if (!this.$v.student.username.$dirty) return errorBag
      !this.$v.student.username.required && errorBag.push('Username is required!')
      return errorBag
    },
    passwordErrorBag () {
      const errorBag = []
      if (!this.$v.student.password.$dirty) return errorBag
      !this.$v.student.password.required && errorBag.push('Password is required!')
      return errorBag
    },
    nameErrorBag () {
      const errorBag = []
      if (!this.$v.student.name.$dirty) return errorBag
      !this.$v.student.name.required && errorBag.push('Name is required!')
      return errorBag
    },
    emailErrorBag () {
      const errorBag = []
      if (!this.$v.student.email.$dirty) return errorBag
      !this.$v.student.email.required && errorBag.push('Name is required!')
      !this.$v.student.email.email && errorBag.push('Email invalid!')
      return errorBag
    }
  },
  methods: {
    create () {
      if (this.isValid) {
        this.axios.post('/students/', this.student)
          .then(() => {
            console.log('Created!')
          })
          .catch(err => {
            console.log(err)
          })
      }
    }
  }
}
</script>
