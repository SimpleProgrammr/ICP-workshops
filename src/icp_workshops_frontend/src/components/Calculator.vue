<script setup>
import { ref, onMounted } from 'vue';
import { icp_workshops_backend } from 'declarations/icp_workshops_backend/index';

let a = ref(null);
let b = ref(null);
let operator = ref("+");
let res = ref("");

async function handleCalculate(e) {
    e.preventDefault();
    //console.log(a.value);
    //console.log(b.value);
    if (a.value === null || b.value === null){
        res.value = "Please enter both numbers";
        return;
    }

    try {
        const response = await icp_workshops_backend.calculate(a.value, b.value, operator.value)
        res.value = response;
        //console.log(res.value);
        localStorage.setItem("res", response)
    } catch (error) {
        res.value = error;
        console.log("Aha");
    }
}

onMounted(() => {
    const storedResult = localStorage.getItem("res");
    //console.log(storedResult);
    if (storedResult) {
        res.value = storedResult;
    }
});
</script>

<template>
    <form class="flex flex-col border-2 p-10 justify-center items-center" action="#" @submit="handleCalculate">
        <label>Enter A: </label>
        <input id="a" v-model.number="a" type="number" placeholder="Number A" class="border-2" />
        <label>Enter B: </label>
        <input id="b" v-model.number="b" type="number" placeholder="Number B" class="border-2"/>
        <label>Choose an operator: </label>
        <select id="operator" v-model="operator" class="border-3">
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
            <option value="%">%</option>
        </select>
        <button type="submit">Calculate</button>
        <p>{{ res }}</p>
    </form>
</template>