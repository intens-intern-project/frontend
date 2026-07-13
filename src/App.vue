<script setup lang="ts">

import axios, { type AxiosResponse } from 'axios';
import { onMounted, ref } from 'vue';

const backendURL = "http://localhost:8080/api";
const error = ref<string | undefined>(undefined);
const counterName = "default";
const counter = ref<number>(0);
const version_api = ref<string>("");

onMounted(() => {
	getCounterValue();
	getApiVersion();
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

const getApiVersion = () => {
	error.value = undefined;
	axios.put(`${backendURL}/version`).then((res: AxiosResponse<string>) => {
		version_api.value = res.data;
	}).catch((err) => {
		error.value = "Failed to fetch";
		console.error(err);
	});
}

</script>

<template>
	<main>
		<h1>IIP</h1>
		API: {{ version_api }} <br/>
		<hr/>
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