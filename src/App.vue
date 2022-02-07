<template>
	<div class="container-fluid">
		<div class="row">
			<h1>Todo List with vue 3 pagination</h1>

			<div class="col">
				<todo></todo>
			</div>
		</div>
	</div>
</template>

<script>
import Todo from './components/Todo.vue';
export default {
	name: 'App',
	components: {
		Todo,
	},
};
</script>
