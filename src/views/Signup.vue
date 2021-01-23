<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Signup</h1>
        <v-form ref="signUpForm" v-model="formValidity">
          <v-text-field label="email" type="email" v-model="email" :rules="emailRules" required></v-text-field>
          <v-autocomplete 
            label="Which browser you use ?"
            :items="browsers"></v-autocomplete>
            <v-file-input label="Attach profile picture"></v-file-input>
            <v-menu
              ref="menu"
              v-model="menu"
              :close-on-content-click="false"
              :return-value.sync="birthday"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  label="Birthday"
                  v-model="birthday"
                  readonly
                  v-bind="attrs"
                  v-on="on"></v-text-field>
              </template>
              <v-date-picker
                ref="picker"
                v-model="birthday"
                prepend-icon="mdi-calendar"
                :max="new Date().toISOString().substr(0, 10)"
                min="1950-01-01"
                @change="save"></v-date-picker>
            </v-menu>
            <v-checkbox label="Agree to terms & conditions" v-model="agreeToTerms" :rules="agreeToTermsRules" required></v-checkbox>
            <v-btn type="submit" color="primary" class="mr-4" :disabled="!formValidity">Submit</v-btn>
            <v-btn color="success" @click="validateForm" class="mr-4">Validate Form</v-btn>
            <v-btn color="warning" @click="resetValidation" class="mr-4">Reset Validation</v-btn>
            <v-btn color="error" @click="resetForms" class="mr-4">Reset</v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    agreeToTerms: false,
    agreeToTermsRules: [
      value =>
        !!value || 'You must agree to the terms and conditions to sign up for an account.'
    ],
    birthday: '',
    menu: false,
    browsers: [
      'Chrome',
      'Firefox',
      'Safari',
      'Edge',
      'Brave'
    ],
    email: '',
    emailRules: [
      value => !!value || 'Email is required.',
      value => value.indexOf('@') !== 0 || 'Email should have a username.',
      value => value.indexOf('@') || 'Email should include an @ symbol.',
      value => value.indexOf('.') - value.indexOf('@') > 1 || 'Email should contain a valid domain',
      value => value.indexOf('.') <= value.length - 3 || 'Email should contain a valid domain extension.'
    ],
    formValidity: false,
  }),
  watch: {
    menu (val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
    },
  },
  methods: {
    resetForms() {
      this.$refs.signUpForm.reset()
    },
    resetValidation () {
      this.$refs.signUpForm.resetValidation()
    },
    save (date) {
      this.$refs.menu.save(date)
    },
    validateForm() {
      this.$refs.signUpForm.validate()
    }
  }
}
</script>