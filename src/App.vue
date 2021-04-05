<template>
	<div class="calc">
		<div>
			<div class="answer-field">{{ current || 0 }}</div>
			<div class="container">
				<div @click="clear" class="spec-item">C</div>
				<div @click="invert" class="spec-item">+/-</div>
				<div @click="percent" class="spec-item">%</div>
				<div @click="() => operator('/')" class="op-item">/</div>
				<div @click="() => addNum('7')" class="num-item">7</div>
				<div @click="() => addNum('8')" class="num-item">8</div>
				<div @click="() => addNum('9')" class="num-item">9</div>
				<div @click="() => operator('x')" class="op-item">x</div>
				<div @click="() => addNum('4')" class="num-item">4</div>
				<div @click="() => addNum('5')" class="num-item">5</div>
				<div @click="() => addNum('6')" class="num-item">6</div>
				<div @click="() => operator('-')" class="op-item">-</div>
				<div @click="() => addNum('1')" class="num-item">1</div>
				<div @click="() => addNum('2')" class="num-item">2</div>
				<div @click="() => addNum('3')" class="num-item">3</div>
				<div @click="() => operator('+')" class="op-item">+</div>
				<div
					style="grid-column: auto / span 2; border-bottom-left-radius: 5px"
					class="num-item"
					@click="() => addNum('0')"
				>
					0
				</div>
				<div @click="addDecimal" class="num-item">.</div>
				<div
					@click="calculate"
					style="border-bottom-right-radius: 5px"
					class="op-item"
				>
					=
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			current: "",
			previous: "",
			opClicked: false,
			op: "",
		}
	},
	methods: {
		clear() {
			this.current = ""
			this.previous = ""
			this.op = ""
			this.opClicked = false
		},
		invert() {
			if (this.current || this.current === "0") {
				if (this.current[0] === "-") {
					this.current = this.current.slice(1, this.current.length)
				} else {
					this.current = "-" + this.current
				}
			}
		},
		percent() {
			if (this.current) {
				this.current = `${parseFloat(this.current) / 100}`
			}
		},
		addNum(num) {
			if (this.opClicked || this.current === "Not a number") {
				this.current = ""
				this.opClicked = false
			}

			this.current += num
		},
		addDecimal() {
			if (this.current) {
				if (this.current.indexOf(".") === -1) {
					this.current += "."
				}
			}
		},
		operator(op) {
			if (this.op) {
				this.calculate()
			}

			this.previous = this.current
			this.op = op
			this.opClicked = true
		},
		calculate() {
			switch (this.op) {
				case "+":
					this.current = `${parseFloat(this.previous) +
						parseFloat(this.current)}`
					break
				case "-":
					this.current = `${parseFloat(this.previous) -
						parseFloat(this.current)}`
					break
				case "x":
					this.current = `${parseFloat(this.previous) *
						parseFloat(this.current)}`
					break
				case "/":
					this.current =
						parseFloat(this.current) !== 0
							? `${parseFloat(this.previous) / parseFloat(this.current)}`
							: "Not a number"
					break
			}
			this.op = ""
			this.opClicked = false
		},
	},
}
</script>

<style scoped>
.calc {
	height: 100vh;
	display: flex;
	justify-content: center;
	align-items: center;
}

.container {
	width: 203px;
	display: grid;
	grid-template-columns: 50px 50px 50px 50px;
	grid-gap: 1px;
	background-color: black;
	color: white;
	border-radius: 5px;
	user-select: none;
}

.spec-item {
	display: flex;
	align-items: center;
	justify-content: center;
	background-color: rgb(52, 52, 52);
	height: 50px;
	cursor: pointer;
}

.spec-item:hover {
	background-color: rgb(89, 88, 88);
}

.num-item {
	display: flex;
	align-items: center;
	justify-content: center;
	background-color: rgb(77, 77, 77);
	height: 50px;
	cursor: pointer;
}

.num-item:hover {
	background-color: rgb(109, 109, 109);
}

.op-item {
	display: flex;
	align-items: center;
	justify-content: center;
	background: orange;
	cursor: pointer;
}

.op-item:hover {
	background: rgb(255, 181, 44);
}

.answer-field {
	display: flex;
	align-items: center;
	justify-content: flex-end;
	width: 193px;
	height: 40px;
	background-color: black;
	color: white;
	font-size: 20px;
	padding-right: 10px;
	border-top-left-radius: 5px;
	border-top-right-radius: 5px;
}
</style>
