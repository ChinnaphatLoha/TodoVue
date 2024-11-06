<script setup>
import { collection, getDocs } from 'firebase/firestore';
import { ref, onMounted } from 'vue';
import db from '@/firebase/init.js';

/**
 * @type {{ id: string, title: string, completed: boolean, timestamp: string }[]}
 */
const todos = ref([]);

async function getToDos() {
	const coll = collection(db, 'todos');
	const querySnapshot = await getDocs(coll);
	querySnapshot.forEach((doc) => {
		todos.value.push(doc.data());
	});
}

onMounted(() => {
	getToDos();
});
</script>

<template>
	<ul>
		<li v-for="todo in todos" :key="todo.title">
			{{ todo.title }}
		</li>
	</ul>
</template>
