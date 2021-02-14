<!-- HTML -->

<template>
	<div class="header">
		<h3 id="text">Todo UI List</h3>
	</div>
	<div class="nav">
		<div>
			<h1>Список задач</h1>
			<form @submit.prevent="addTodo()">
				<label>Добавление задачи</label>
				<input
					v-model="newTodo"
					name="newTodo"
					autocomplete="off"
				>
			</form>
			<h2>Список</h2>
			<ul id="task_done">
				<li
					v-for="(todo, index) in todos"
					:key="index"
				>
					<span
						:class="{ done: todo.done }"
						@click="doneTodo(todo)"
					>{{ todo.content }}</span>
					<button @click="removeTodo(index)" id="remove_btn">❌</button>
				</li>
			</ul>
			<h4 v-if="todos.length === 0">Список пуст.</h4>
		</div>
	</div>
	<div class="nav_nts">

	</div>
</template>

<!-- VUE-JS-->
<script>
	import { ref } from 'vue';
	export default {
		name: 'App',
		setup () {
			const newTodo = ref('');
			const defaultData = [{
				done: false,
				content: ''
			}]
			const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
			const todos = ref(todosData);
			function addTodo () {
				if (newTodo.value) {
					todos.value.push({
						done: false,
						content: newTodo.value
					});
					newTodo.value = '';
				}
				saveData();
			}

			function doneTodo (todo) {
				todo.done = !todo.done
				saveData();
			}

			function removeTodo (index) {
				todos.value.splice(index, 1);
				saveData();
			}

			function saveData () {
				const storageData = JSON.stringify(todos.value);
				localStorage.setItem('todos', storageData);
			}

			return {
				todos,
				newTodo,
				addTodo,
				doneTodo,
				removeTodo,
				saveData
			}
		}
	}
</script>

<!-- CSS -->
<style lang="scss">
body {
	margin: 0;
	padding: 0;
}
.header {
	display: flex;
	background-color: rgb(0, 0, 0);
	height: 100px;
	color: white;
	text-align: center;
}
#text {
	display: block;
	margin-left: auto;
	margin-right: auto;
	font-weight: bold;
	font-size: 30px;
	text-align: center;
}
/* Navigation */
.nav{
	overflow-x: hidden;
	border-right: 2px solid black;
	width: 400px;
	height: 869px;
}
h1 {
	font-weight: bold;
	font-size: 28px;
	text-align: center;
}
form {
	text-align: center;
	display: flex;
	flex-direction: column;
	width: 80%;
	margin: 0 auto;
	label {
		font-size: 14px;
		font-weight: bold;
	}
	input,
	button {
		height: 45px;
		box-shadow: none;
		outline: none;
		padding-left: 12px;
		padding-right: 12px;
		border-radius: 6px;;
		font-size: 18px;
		margin-top: 6px;;
		margin-bottom: 12px;
	}
	input {
		background-color: transparent;
		border: 2px solid white 0.35;	
		color: inherit;
	}
}
button {
	cursor: pointer;
	background-color: #a0a4d9;
	border: 1px solid #a0a4d9;
	color: #1f2023;
	font-weight: bold;
	outline: none;
	border-radius: #1f2023;
}
h2 {
	text-align: center;
	font-size: 22px;
	border-bottom: 2px solid white 0.35;
	padding-bottom: 6px;
}
ul {
	padding: 10px;
	li {
		display: flex;
		justify-content: space-between;
		align-items: center;
		border: 2px solid white 0.35;
		padding: 12px 24px;
		border-radius: 6px;
		margin-bottom: 12px;
		span {
			cursor: pointer;
			font-size: 20px;
			color: rgb(48, 47, 47);
		}
		.done {
			text-decoration: line-through;
		}
		button {
			font-size: 12px;
			padding: 6px;
		}
	}
}
ul li:hover {
	cursor: pointer;
	border-radius: 4px;
    background-color: #dbdbdb;
}
#remove_btn {
    width: 12%;
    border: none;
    margin-left: 20px;
    background-color: unset;
}
#remove_btn:hover {
	cursor: pointer;
    border-radius: 4px;
    background-color: #c014145b;
}
h4 {
	text-align: center;
	opacity: 0.5;
	margin: 0;
}

/* Navigation-notes */
.nav_nts{
	border-left: 2px solid black;
	margin-top:-870px;
	margin-left: 420px;
	max-width: 100%;
	height: 869px;
}
</style>