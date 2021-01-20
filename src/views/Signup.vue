<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Signup</h1>
        <v-form>
          <v-text-field label="email" type="email"></v-text-field>
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
            <v-checkbox label="Agree to terms & conditions"></v-checkbox>
            <v-btn type="submit" color="primary">Submit</v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    birthday: '',
    menu: false,
    browsers: [
      'Chrome',
      'Firefox',
      'Safari',
      'Edge',
      'Brave'
    ]
  }),
  watch: {
    menu (val) {
      val && setTimeout(() => (this.$refs.picker.activePicker = 'YEAR'))
    },
  },
  methods: {
    save (date) {
      this.$refs.menu.save(date)
    },
  }
}
</script>