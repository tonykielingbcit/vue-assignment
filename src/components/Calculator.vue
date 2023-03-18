<script>
    import Display from './Display.vue';
    import ButtonsGrid from "./ButtonsGrid.vue";

    export default {
        components: {
            Display, ButtonsGrid
        },
        data() {
            return {
                content: 0,
                valueInMemory: 0,
                tempData: "",
                currentOperator: "",
                percent: "",
                startTyping: false,
            }
        },
        methods: {
            evaluate(input) {
                return new Function("return " + input)();
            },

            receivingData({type, value, text}) {
                switch (type){
                    case "number":
                        this.content = (!this.content || this.startTyping) ? value : this.content + "" + value;
                        this.startTyping = false;

                        break;


                    case "operator":
                        this.percent = "";
                        
                        if (this.startTyping) {
                            this.currentOperator = value;
                        } else if (this.tempData) {
                            const toEval = `${this.tempData} ${this.currentOperator} ${this.content}`
                            const result = this.evaluate(toEval);
                            this.content = result;
                            this.currentOperator = value;
                            this.tempData = result;
                        } else {
                            this.currentOperator = value;
                            this.tempData = this.content;
                        }

                        this.startTyping = true;

                        break;
            

                    case "clear":
                        this.content = 0;
                        this.percent = "";
                        
                        if (text === "AC") {
                            this.tempData = "";
                            this.currentOperator = "";
                            this.valueInMemory = "";
                        } else {
                            if (!this.currentOperator)
                                this.tempData = "";
                        }

                        break;
        

                    case "enter":
                        if (this.percent) {
                            const toEval = `${this.tempData} ${this.currentOperator} ${this.content}`
                            const result = this.evaluate(toEval);
                            this.content = result;
                        } else if (this.tempData !== "" && this.content !== "" && this.currentOperator !== "") {
                            const toEval = `${this.tempData} ${this.currentOperator} ${this.content}`
                            const result = this.evaluate(toEval);
                            this.content = result;
                            this.tempData = result;
                            this.startTyping = true;

                            if (!this.percent)
                                this.currentOperator = "";
                        }

                        break;


                    case "memory":
                        if (text === "MS") {
                            this.valueInMemory = this.content;
                            this.content = "";
                            this.tempData = "";
                            this.currentOperator = "";
                        }
                        else if (text === "MC")
                            this.valueInMemory = "";
                        else if (text === "MR") {
                            this.content = this.valueInMemory;
                            if (!this.currentOperator)
                                this.tempData = this.valueInMemory;
                        } else if (text === "M+") {
                            const toEval = `${this.content} + ${this.valueInMemory}`
                            const result = this.evaluate(toEval);
                            this.content = result;
                        } else if (text === "M-") {
                            const toEval = `${this.content} - ${this.valueInMemory}`
                            const result = this.evaluate(toEval);
                            this.content = result;
                        }

                        break;


                    case "sqrt":
                        const temp = Math.sqrt(this.content);
                        this.content = temp;
                        this.tempData = "";
                        this.currentOperator = "";
                        
                        break;


                    case "sign":
                        this.content = this.content * -1;
                        this.tempData = this.content;
                        break;


                    case "percent":
                        const k = this.tempData * 0.01;
                        this.percent = k;
                        this.content = k;

                        break;


                    default: 
                        break
                }
            }
        }
    }
</script>

<template>
    <main class="App-header">
        <div class="frame">
            <h3 class = "calc-title">Vue Calculator</h3>
            <div class = "display-aux">
              <span class = "memory-data">{{ valueInMemory ? "(M)" + valueInMemory : "" }}</span>
              <span class = "temp-data">{{ tempData }} {{ currentOperator }} {{ percent }} </span>
            </div>
            <Display :toDisplay = "content" />
            <ButtonsGrid @sendDataToCalculator="receivingData"/>
        </div>
    </main>
</template>
