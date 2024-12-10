<template>
    <div class="container d-flex justify-content-center align-items-center vh-100">
        <div class="calculator bg-dark rounded shadow p-3">
            <!-- Display -->
            <div class="display bg-black text-white text-end rounded p-2 mb-3">
                <h3 class="m-0">{{ current }}</h3>
            </div>
            <!-- Buttons -->
            <div class="grid">
                <button class="btn btn-light-gray" @click="clear">C</button>
                <button class="btn btn-dark" disabled></button>
                <button class="btn btn-dark" disabled></button>
                <button class="btn btn-blue" @click="appendOperation('/')">÷</button>

                <button v-for="n in 3" :key="'7to9' + n" class="btn btn-light-gray" @click="appendNumber(n + 6)">{{ n +
                    6 }}</button>
                <button class="btn btn-red" @click="appendOperation('*')">×</button>

                <button v-for="n in 3" :key="'4to6' + n" class="btn btn-light-gray" @click="appendNumber(n + 3)">{{ n +
                    3 }}</button>
                <button class="btn btn-red" @click="appendOperation('-')">−</button>

                <button v-for="n in 3" :key="'1to3' + n" class="btn btn-light-gray" @click="appendNumber(n)">{{ n
                    }}</button>
                <button class="btn btn-red" @click="appendOperation('+')">+</button>

                <button class="btn btn-light-gray" @click="appendNumber(0)" :style="{ gridColumn: 'span 2' }">0</button>
                <button class="btn btn-light-gray" @click="appendNumber('.')">.</button>
                <button class="btn btn-orange" @click="calculate">=</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            current: '0',
            previous: null,
            operator: null,
        };
    },
    methods: {
        appendNumber(number) {
            if (this.current === '0' && number !== '.') {
                this.current = `${number}`;
            } else {
                this.current += `${number}`;
            }
        },
        appendOperation(op) {
            if (this.current !== '0') {
                this.previous = this.current;
                this.operator = op;
                this.current = '0';
            }
        },
        calculate() {
            if (this.previous !== null && this.operator !== null) {
                const prev = parseFloat(this.previous);
                const curr = parseFloat(this.current);
                switch (this.operator) {
                    case '+':
                        this.current = `${prev + curr}`;
                        break;
                    case '-':
                        this.current = `${prev - curr}`;
                        break;
                    case '*':
                        this.current = `${prev * curr}`;
                        break;
                    case '/':
                        this.current = curr !== 0 ? `${prev / curr}` : 'Erro';
                        break;
                }
                this.previous = null;
                this.operator = null;
            }
        },
        clear() {
            this.current = '0';
            this.previous = null;
            this.operator = null;
        },
    },
};
</script>

<style scoped>
.calculator {
    width: 320px;
}

.grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
}

button {
    height: 60px;
    font-size: 1.5rem;
    border: none;
    border-radius: 8px;
    font-weight: bold;
}

.display {
    height: 70px;
    font-size: 2rem;
    font-family: 'Courier New', Courier, monospace;
    overflow: hidden;
    text-overflow: ellipsis;
}

/* Button Colors */
.btn-light-gray {
    background-color: #aaa;
    color: black;
}

.btn-light-gray:hover {
    background-color: #ccc;
}

.btn-red {
    background-color: #d32f2f;
    color: white;
}

.btn-red:hover {
    background-color: #f44336;
}

.btn-blue {
    background-color: #1976d2;
    color: white;
}

.btn-blue:hover {
    background-color: #2196f3;
}

.btn-orange {
    background-color: #ff9800;
    color: white;
}

.btn-orange:hover {
    background-color: #ffb74d;
}

/* Background Colors */
body {
    background-color: #2c2c2c;
    color: white;
}
</style>