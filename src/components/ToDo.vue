<script setup>
import { query, collection, getDocs, Timestamp } from 'firebase/firestore';
import { ref, onMounted, } from 'vue';
import db from '@/firebase/init.js';

/**
 * @type { import('vue').Ref<{ id: string, title: string, completed: boolean, createdAt: Timestamp }[]> }
 */
const todos = ref([]);

async function getToDos() {
	const coll = collection(db, 'todos');
	const querySnapshot = await getDocs(query(coll));
	querySnapshot.forEach((doc) => {
		const id = doc.id;
		todos.value.push({ id, ...doc.data() });
	});
}

onMounted(() => {
	getToDos();
});
</script>

<template>
	<ul>
		<li
			v-for="todo in todos"
			:key="todo.title"
		>
			{{ todo.title }}
		</li>
	</ul>
</template>
