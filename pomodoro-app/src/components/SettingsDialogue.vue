<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <v-card>
      <v-card-title>
          <span class="headline">Settings</span>
      </v-card-title>
      <v-card-text>
          <v-text-field
            label="Pomodoro"
            v-for="(timer, i) in updatedTimers"
            v-model="updatedTimers[i]"
            :key="i"
            type="number">
          </v-text-field>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="closeDialog">Close</v-btn>
        <v-btn color="blue darken-1" text @click="save(updatedTimers)">Safe</v-btn>
    </v-card-actions>
    </v-card>

  </v-dialog>
</template>

<script>
  export default {
    props: {
      dialog: {
        type: Boolean,
        require: true
      },
    closeDialog: {
      type: Function,
      required: true
      },
      timers: {
        type: Array,
        required: true
      },
      save: {
        type: Function,
        required: true

      }
    },
    data() {
      return {
              updatedTimers: []
      }

    },
    mounted() {
      this.updatedTimers = this.timers.map((timer) => {
        return timer.minutes
      })
    }
  }
</script>