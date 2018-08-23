<template>
	<div id="app" class="container">
		<TodoList></TodoList>
	</div>
</template>

<script>

	import TodoList from './components/TodoList.vue'

	export default {
		name: 'app',
		components: {
			TodoList,
		}
	}
</script>

<style scoped>

	* {
		box-sizing: border-box;
	}

	.container {
		max-width: 600px;
		margin: 0 auto;
	}

	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		/*text-align: center;*/
		color: #6b6064;
		margin-top: 60px;
		font-size: 18px;
	}

</style>
