<script>
export default {
    name: 'Counter',
    props: {
        initialCount: {
            type: Number,
            default: 0
        },
    },
    data() {
        return {
            count: this.initialCount,
            warning: "",
            color: "text-green-500"
        }
    },
    methods: {
        increment() {
            this.count++;
            if (this.count > 35) {
                this.warning = "Error: Max threshold ⛔. Any more clicks and you may die!";
                this.color = "text-red-500";
            }
            else if (this.count > 15 && this.count <= 35) {
                this.warning = "Warning: Exceeding threshold ⚠️";
                this.color = "text-yellow-500";
            }
            else {
                this.warning = "";
                this.color = "text-green-500";
            }
        },
        decrement() {
            if (this.count > 0) {
                this.count--;
            }
            if (this.count <= 15) {
                this.warning = "";
                this.color = "text-green-500";
            }
            else if (this.count > 15 && this.count <= 35) {
                this.warning = "Warning: Exceeding threshold ⚠️";
                this.color = "text-yellow-500";
            }
            else {
                this.warning = "Error: Max threshold ⛔. Any more clicks and you may die!";
                this.color = "text-red-500";
            }
        }
    }
}
</script>

<template>
    <div class="counter">
        <h2>The current count is <span :class="color">{{count}}</span></h2>
        <div class="flex p-1">
            <button class="text-white " @click="increment" :disabled="count > 50">+</button>
            <button class="text-white" @click="decrement" :disabled="count == 0">-</button>
        </div>
    </div>
    <div class="message flex items-center justify-center">
        <h2>{{warning}}</h2>
    </div>
</template>

<style scoped>
.counter {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
}

.message {
    color: #ccc;
}

h2 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
    color: rgb(196, 196, 196);
}

button {
    background-color: rgb(0, 0, 0);
    border: 1px solid #ccc;
    border-radius: 0.25rem;
    cursor: pointer;
    font-size: 1rem;
    padding: 0.5rem 1rem;
    margin: 1rem;

}

button:active {
    background-color: rgb(215, 153, 209);
}
</style>