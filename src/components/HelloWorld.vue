<template>
	<div class="hello">
		<h1>{{ msg }}</h1>

	</div>
</template>

<script>
	export default {
		name: 'HelloWorld',
		props: {
			msg: String,

		},
		data() {
			return {
				calendar: [],
				theTimeStamp: 0,
				oneDayTimeStamp: 86400000,
				calendarDays: 30,
				timeStamps: [],
				computer: {
					"id": 177,
					"itcode": "songdan7",
					"sutName": "ThinkSystem SR850-un-signed",
					"data": [{
						"backgroud_type": null,
						"enddatetimeStr": "2021-01-30",
						"id": 177,
						"itcode": "songdan7",
						"machine_status": "out",
						"startdatetimeStr": "2021-01-30",
						"sutName": "ThinkSystem SR850-un-signed"
					}, {
						"backgroud_type": null,
						"enddatetimeStr": "2021-02-25",
						"id": 177,
						"itcode": "songdan7",
						"machine_status": "other",
						"startdatetimeStr": "2021-02-23",
						"sutName": "ThinkSystem SR850-un-signed"
					}, {
						"backgroud_type": null,
						"enddatetimeStr": "2021-02-21",
						"id": 177,
						"itcode": "songdan7",
						"machine_status": "book",
						"startdatetimeStr": "2021-02-01",
						"sutName": "ThinkSystem SR850-un-signed"
					}]
				}

			}
		},
		methods: {

			calculation() {
				//data[j].data
				var dayToColor = new Array(this.calendarDays).fill(0)
				for (var i = 0; i < this.computer.data.length; i++) {
					this.calculationColor(this.computer.data[i].startdatetimeStr, this.computer.data[i].enddatetimeStr, this.computer.data[
							i].machine_status,
						dayToColor)
				}
				this.computer.dayToColor = dayToColor
			},

			calculationColor(start, stop, type, dayToColor) {

				start = new Date(new Date(start).toLocaleDateString()).getTime()
				if (stop === null || stop === "") {
					stop = start
				}
				stop = new Date(new Date(stop).toLocaleDateString()).getTime()
				var startIndex = this.timeStamps.indexOf(start)
				var stopIndex = this.timeStamps.indexOf(stop)
				//不在日历内
				if (startIndex < 0 && stopIndex < 0) { //都不在日历内
					var calendarStop = this.calendar[this.calendarDays - 1].timeStamp
					if (start < calendarStop < stop) { //全在日历内
						startIndex = 0
						stopIndex = this.calendarDays - 1
					} else {
						return //过期
					}
				}
				if (startIndex < 0) { //开始不在日历内
					startIndex = 0
				} else if (stopIndex < 0) {
					stopIndex = this.calendarDays - 1
				}

				var arrLen = stopIndex - startIndex + 1
				var arr = new Array(arrLen)
				if (type === 'book') {
					arr.fill('red')
				} else if (type === 'out') {
					arr.fill('black')
				} else { //borrowed
					arr.fill('orange')
				}

				dayToColor.splice(startIndex, arrLen, ...arr)

			},

			getCalendar() {
				this.theTimeStamp = new Date(new Date().toLocaleDateString()).getTime() //时间置为00:00:00
				this.creatCalendar(this.theTimeStamp, this.calendarDays)
			},
			creatCalendar(todayTimeStamp, length) {
				for (var i = 0; i < length; i++) {
					var timeStamp = todayTimeStamp + this.oneDayTimeStamp * i
					var date = new Date(timeStamp)
					this.timeStamps.push(timeStamp)
					this.calendar.push(date)
				}
			}

		},
		created() {
			this.calendarDays = 33
			this.getCalendar()
			console.log(this.calendar)
			this.calculation()
			console.log(this.computer)
		},
		mounted() {}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	h3 {
		margin: 40px 0 0;
	}

	ul {
		list-style-type: none;
		padding: 0;
	}

	li {
		display: inline-block;
		margin: 0 10px;
	}

	a {
		color: #42b983;
	}
</style>
