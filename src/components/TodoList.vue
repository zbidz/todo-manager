<template>
	<div id="TodoList">
		<input type="text" class="todo-input" placeholder="Новая задача" v-model="newTodo" @keyup.enter="addTodo">
		<button
			class="add-todo-btn" v-on:click="addTodo">+
		</button>


		<div class="extra-container">
			<div>
				<button :class="{ active: filter === 'all' }" @click="filter = 'all'">Все</button>
				<button :class="{ active: filter === 'active' }" @click="filter = 'active'">Активные</button>
				<button :class="{ active: filter === 'completed' }" @click="filter = 'completed'">Завершенные</button>
			</div>
			<div>
				<button class="delete-completed-btn" v-if="showBtnDeleteCompleted" @click="deleteCompleted">Удалить завершенные
				</button>
			</div>
		</div>

		<div class="extra-container">
			<div><label><input id="all-check" type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Выбрать все
			</label></div>
			<div>{{ remaining }} Актуально</div>
		</div>

		<TodoItem :items="todosFiltered"></TodoItem>

	</div>
</template>

<script>

	import TodoItem from './TodoItem.vue'


	export default {
		name: 'TodoList',
		components: {
			TodoItem
		},

		data() {
			return {
				newTodo: '',
				date: '',
				filter: 'all',
				todos: [
					{
						'title': 'Тестовая задача',
						'completed': false,
						'editing': false,
						'date': this.date
					},
					{
						'title': '2 Тестовая задача',
						'completed': false,
						'editing': false,
						'date': this.date
					},
				],
			}
		},

		computed: {
			remaining() {
				return this.todos.filter(todo => !todo.completed).length
			},

			anyRemaining() {
				return this.remaining !== 0
			},

			todosFiltered() {
				if (this.filter ==='all')
					return this.todos;
				else if (this.filter === 'active')
					return this.todos.filter(todo => !todo.completed);
				else if (this.filter === 'completed')
					return this.todos.filter(todo => todo.completed);

				return this.todos
			},

			showBtnDeleteCompleted() {
				return this.todos.filter(todo => todo.completed).length > 0
			}
		},

		methods: {
			addTodo() {
				if (this.newTodo.trim().length === 0) {
					return
				}

				this.date = new Date();

				let day = this.date.getDate();
				let month = this.date.getMonth();
				let year = this.date.getFullYear();
				let hours = this.date.getHours();
				let minutes = this.date.getMinutes();
				let seconds = this.date.getSeconds();

				if (day < 10) day = '0' + day;
				if (month < 10) month = '0' + month;
				if (hours < 10) hours = '0' + hours;
				if (seconds < 10) seconds = '0' + seconds;

				this.date = 'Добавлена: ' + day + '.' + month + '.' + year + '  ' + ' ' + ' в ' + hours + ':' + minutes +
					':' + seconds;

				this.todos.unshift({
					title: this.newTodo,
					completed: false,
					editing: false,
					date: this.date
				});

				this.newTodo = ''
			},

			checkAllTodos() {
				this.todos.forEach((todo) => todo.completed = event.target.checked)
			},

			deleteCompleted () {
				this.todos = this.todos.filter(todo => !todo.completed)
			}

		}
	}
</script>

<style scoped lang="scss">
	.todo-input {
		width: 510px;
		padding: 10px 18px;
		font-size: 18px;
		margin-bottom: 20px;

		&:focus {
			outline: none;
		}
	}

	.add-todo-btn {
		width: 50px;
		height: 45px;
		position: absolute;
		font-size: 34px;
		color: grey;
		margin-left: -1px;
	}

	.extra-container {
		display: flex;
		align-items: center;
		justify-content: space-between;
		font-size: 14px;
		border-bottom: 1px solid lightgray;
		padding-bottom: 14px;
		margin-bottom: 14px;
	}

	button {
		border: 1px solid grey;
		margin-left: 5px;
		font-size: 14px;
		color: grey;
		background-color: white;

		&:hover {
			cursor: pointer;
			background: grey;
			color: white;
			transition: 0.3s;
		}

		&:focus {
			outline: none;
		}

	}

	.active {
		border: 1px solid lightgreen;
		background: lightgreen;
		color: white;
	}

	.delete-completed-btn {
		border: 1px solid lightcoral;

		&:hover {
			background: lightcoral;
			transition: 0.3s;
			color: white;

		}

	}


</style>
