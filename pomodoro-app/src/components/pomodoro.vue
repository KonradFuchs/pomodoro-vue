<template>
	<v-card class="mt-8">
		<v-tabs @change="changeCurrentTimer" v-model="currentTimer" grow>
			<v-tab 
				v-for="timer in timers"
				:key="timer.name"
				>
				{{ timer.name }}
			</v-tab>
			<v-tabs-items v-model="currentTimer">
				<v-tab-item> 
					
				</v-tab-item>
			</v-tabs-items>
		</v-tabs>
		<v-card

				flat
				class="pa-5 d-flex flex-column justify-ceter align-center">
				<h1 class="time"> {{displayMinutes}}:{{displaySeconds}}</h1>
					<div class="button-group">
						<v-btn @click="start" color="primary"><v-icon> mdi-play-circle-outline</v-icon>Start</v-btn>
						<v-btn @click="stop" color="error"><v-icon>mdi-stop-circle-outline</v-icon>Stop</v-btn>
						<v-btn @click="reset(timers[currentTimer].minutes)" :disabled="isRunning"><v-icon>mdi-restart</v-icon>Reset</v-btn>
						<v-btn @click="dialog = true" color="grey" dark><v-icon>mdi-cog-outline </v-icon>
						</v-btn>
					</div>					
				</v-card>
			<SettingsDialog :dialog="dialog" :closeDialog="closeDialog" :save="save" :timers="timers" />
	</v-card>
</template>

<script>
import SettingsDialog from './SettingsDialogue.vue'

export default {
	components: {
		SettingsDialog
	},
	data () {
	return {
		isRunning: false,
		timerInstance: null,
		display: {
			minutes: '00',
			seconds:'00',
		},
		totalSeconds: 25 * 60,
		currentTimer: 0,
		timers: [{
			name:'Pomodoro',
			minutes: 25,
			seconds: 0,
		},
		{
			name:'Short Break',
			minutes: 5,
			seconds: 0,
		},
		{	
			name:'Long Break',
			minutes: 10,
			seconds: 0,
			}],
			dialog: false
		}
	},
	computed: {
		displayMinutes() {
			const minutes = Math.floor(this.totalSeconds / 60)
			return this.formatTime(minutes)
		},
		displaySeconds() {
			let seconds = this.totalSeconds % 60

			return this.formatTime(seconds)
		}
		
	},
	methods: {
		formatTime(time)  {
		if (time < 10) {
		return '0' + time
			}	
			return time.toString()
		},
		start () {
			
			this.stop()
			this.isRunning = true
			this.timerInstance = setInterval(() => {
				if (this.totalSeconds <= 0) {
					this.stop()
					return
				}
				this.totalSeconds -= 1
			}, 1000)
		},
		stop () {
			clearInterval(this.timerInstance)
			this.isRunning = false
		},
		reset (minutes) {
			this.stop()
			this.totalSeconds= minutes * 60
		},
		changeCurrentTimer (num) {
			this.currentTimer = num
			this.reset(this.timers[num].minutes)
		},
		closeDialog() {
			this.dialog = false
		},
    save(updatedTimers) {
      this.timers = this.timers.map((timer, i) => {
        return { ...timer, minutes: parseInt(updatedTimers[i]) }
      })
      this.closeDialog()
    }
  }
}
</script>

<style lang="sass" scoped>
.v-card
	widht:600px
.time
	font-size: 80px
	font-weight: 400
	text-aligh: center
.v-btn 
	margin: 0 2px

</style>