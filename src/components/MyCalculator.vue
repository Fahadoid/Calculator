<template>
    <div class = "calculator" >
        <div class = "display-total">{{ expression || "Enter a value" }} </div>
        <div class = "display-current">{{ current || 0 }}  </div>
        <section class = "buttons">
            <div class = "btn-row">
                <button @click="clear" class = "btn">AC</button>
                <button @click="sign" class = "btn">&plus;/&minus;</button>
                <button @click="percent" class = "btn">%</button>
                <button @click="divide" class = "btn operator">&divide;</button>
            </div>
            <div class = "btn-row">
                <button @click=append(7) class = "btn">7</button>
                <button @click=append(8) class = "btn">8</button>
                <button @click=append(9) class = "btn">9</button>
                <button @click="multiply" class = "btn operator">&times;</button>
            </div>
            <div class = "btn-row">
                <button @click=append(4) class = "btn">4</button>
                <button @click=append(5) class = "btn">5</button>
                <button @click=append(6) class = "btn">6</button>
                <button @click="minus" class = "btn operator">-</button>
            </div>
            <div class = "btn-row">
                <button @click=append(1) class = "btn">1</button>
                <button @click=append(2) class = "btn">2</button>
                <button @click=append(3) class = "btn">3</button>
                <button @click="add" class = "btn operator">+</button>
            </div>
            <div class = "btn-row">
                <button @click=append(0) class = "btn zero">0</button>
                <button @click="decimal" class = "btn">.</button>
                <button @click="equal" class = "btn operator">&equals;</button>
            </div>
        </section>
               
    </div>
</template>

<script>

export default {
    data() {
        return {
            current: '',
            operator: null,
            previous: null,
            operatorClicked: false,
            expression: '',
        }
    },

    methods: {
        clear() {
            this.current = '';
            this.expression = '';
        },

        sign() {
            this.current = this.current.charAt(0) === '-' ? 
            this.current.slice(1) : `-${this.current}`;
        },

        percent() {
            this.current = `${parseFloat(this.current) / 100}`
        },

        append(number) {
            if (this.operatorClicked) {
            this.current = '';
            this.operatorClicked = false;
        }
        
        this.current = `${this.current}${number}`;
        this.expression += number;
            
        },

        decimal() {
            if (this.current.indexOf('.') === -1) {
                this.append('.')
            }
        },

        divide() {
            this.setOperator((a, b) => a / b, '÷');
        },

        multiply() {
            this.setOperator((a, b) => a * b, '×');
        },

        minus() {
            this.setOperator((a, b) => a - b, '-');
        },
        add() {
            this.setOperator((a, b) => a + b, '+');
        },

        square() {
            const currentValue = parseFloat(this.current);
            this.current = `${Math.pow(currentValue, 2)}`;
            this.expression += `sqr(${currentValue})`
        },

        equal() {
            this.operate();
            this.operator = null;
            this.previous = null;
            this.expression = this.current;
        },

        operate() {
            if (this.operator && this.previous && this.current !== '') {
                this.current = `${this.operator(parseFloat(this.previous), parseFloat(this.current))}`;
                this.previous = this.current;
                this.operatorClicked = true;
            }
        },

        setPrevious() {
            if (this.operatorClicked) {
                this.operate();
            } else {
                this.previous = this.current;
                this.operatorClicked = true;
            }
        },

        setOperator(operatorFn, symbol) {
            if (this.current !== '') {
                if (this.previous !== null && !this.operatorClicked) {
                    this.operate();
                    this.expression += ` ${symbol} `;
                } else {
                    this.expression = `${this.current} ${symbol} `;
                    this.previous = this.current;
                }
            this.operator = operatorFn;
            this.operatorClicked = true;
            this.current = '';
            }
        },

    }

}
</script>

<style scoped>
.calculator {
    
    margin: 50px auto; 
    background: #0e151c;
    justify-content: center;
    align-items: center;
    display: flex;
    flex-direction: column;
    height: 100vh;
    width: 190vh;


}

.display-current {
    text-align: center;
    margin: 10px;
    background-color: #31475e;
    border: 25px solid #223344;
    border-radius: 25px;
    border-left-width: 10rem;
    border-right-width: 10rem;
    color: white;
    
}

.display-total {
    text-align: center;
    margin: 10px;
    background-color: #31475e;
    border: 25px solid #223344;
    border-radius: 25px;
    border-left-width: 100px;
    border-right-width: 100px;
    color: white;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    
}

.btn {
    color: white;
    background-color: #223344;
    border: 25px solid #223344;
    border-radius: 25px;
    cursor: pointer;
}

.btn:hover {
    cursor: pointer;
}

.zero {

}

.operator {


color: white;
}

</style>
