<template>
	<div>
		<div v-for="(item, index) in items" :key="index" class="todo-item">
			<div class="todo-item-left">
				<input type="checkbox" v-model="item.completed">
				<div v-if="!item.editing" @dblclick="editTodo(item)" class="todo-item-label" :class="{ completed : item.completed}">
					<span>{{item.title}}</span>
					<div class="todo-date-create"><span>{{item.date}}</span></div>
				</div>
				<input v-else class="todo-item-edit" type="text" v-model="item.title" @blur="doneEdit(item)" @keyup.enter="doneEdit(item)" @keyup.esc="cancelEdit(item)" v-focus>
			</div>
			<div class="remove-item" @click="removeItem(index)">
				&times;
			</div>
		</div>
	</div>
</template>

<script>

	export default {
		beforeEditCache: '',
		props: ['items'],
		directives: {
			focus: {
				// определение директивы
				inserted: function (el) {
					el.focus()
				}
			}
		},

		methods: {

			editTodo(item) {
				this.beforeEditCache = item.title;
				item.editing = true
			},

			cancelEdit(item) {
				item.title = this.beforeEditCache;
				item.editing = false
			},

			doneEdit(item) {
				if (item.title.trim() === '') {
					item.title = this.beforeEditCache
				}
				item.editing = false
			},

			removeItem(index) {
			this.items.splice(index, 1)

			}
		},

	}


</script>

<style scoped lang="scss">
	.todo-item {
		width: 600px;
		margin-bottom: 12px;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.remove-item {
		cursor: pointer;
		font-size: 28px;

		&:hover {
			color: red;
		}
	}

	.todo-item-left {
		display: flex;
		align-items: center;
	}

	.todo-item-label {
		padding: 10px;
		max-width: 600px;
		border: 1px solid white;
		height: 20px;
	}

	.todo-date-create {
		font-size: 12px;
	}

	.todo-item-edit {
		font-size: 24px;
		color: #2c3e50;
		width: 100%;
		padding: 10px;
		border: 1px solid #ccc;

		&:focus {
			outline: none;
		}
	}

	.completed {
		text-decoration: line-through;
		color: #cecece;
	}
</style>