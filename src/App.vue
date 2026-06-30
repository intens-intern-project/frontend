<script setup lang="ts">

import axios, { type AxiosResponse } from 'axios';
import { onMounted, ref } from 'vue';

const backendURL = "http://localhost:8080";
const error = ref<string | undefined>(undefined);
const counterName = "default";
const counter = ref<number>(0);

onMounted(() => {
	getCounterValue();
});

interface CounterDTO {
	id: number;
	name: string;
	value: number;
}

const getCounterValue = () => {
	error.value = undefined;
	axios.get(`${backendURL}/counter`).then((res: AxiosResponse<CounterDTO>) => {
		counter.value = res.data.value;
	}).catch((err) => {
		error.value = "Failed to fetch";
		console.error(err);
	});
}

const incrementCounter = () => {
	error.value = undefined;
	axios.put(`${backendURL}/counter/plus`).then((res: AxiosResponse<CounterDTO>) => {
		counter.value = res.data.value;
	}).catch((err) => {
		error.value = "Failed to fetch";
		console.error(err);
	});
}

const resetCounter = () => {
	error.value = undefined;
	axios.put(`${backendURL}/counter/reset`).then((res: AxiosResponse<CounterDTO>) => {
		counter.value = res.data.value;
	}).catch((err) => {
		error.value = "Failed to fetch";
		console.error(err);
	});
}

</script>

<template>
	<main>
		<b>Counter value</b>: {{  counter }}
		<br /><br/>

		<button @click="incrementCounter">Increment</button>
		<button @click="resetCounter">Reset</button>
		<br/>
		<div v-if="error" class="error">{{ error }}</div>
	</main>
</template>

<style lang="css" scoped>
.error {
	color: red;
}

button {
	margin: 1em;
}
</style>