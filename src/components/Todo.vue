<template>
	<Preloader v-show="loading" />
	<div class="card" v-show="!loading">
		<div class="card-body">
			<div class="table-responsive">
				<table class="table">
					<thead>
						<tr>
							<th scope="col">#</th>
							<th scope="col">User ID</th>
							<th scope="col">Title</th>
							<th scope="col">Completed</th>
							<th></th>
						</tr>
					</thead>
					<tbody data-bs-spy="scroll">
						<tr v-for="(todo, index) in todos" :key="todo.id">
							<td>{{ per_page * (page - 1) + ++index }}</td>
							<td>{{ todo.userId }}</td>
							<td>{{ todo.title }}</td>
							<td>{{ todo.completed }}</td>
							<td></td>
						</tr>
					</tbody>
				</table>
			</div>
			<div class="row text-end text-sm-start">
				<div class="col-md-6 offset-md-6 text-muted fs-6">
					<div class="row d-flex justify-content-center">
						<div class="col-md-4">
							<span class="">
								row per page
								<select
									class="form-control-sm border-0 text-muted p-0 m-0"
									style="background: none; outline: 0"
									@change="changePageSize"
								>
									<option value="10">10</option>
									<option value="25">25</option>
									<option value="50">50</option>
									<option value="100">100</option>
								</select>
							</span>
						</div>
						<div class="col-md-8 col-sm-12">
							<div class="row d-flex">
								<div class="col-md-4 col-sm-6 offset-md-2 offset-sm-0 w-50">
									<small
										>{{ (page - 1) * per_page + 1 }} - {{ per_page * page }} of
										{{ total }}</small
									>
								</div>
								<div class="col-md-4 col-sm-6">
									<div class="row">
										<div class="col-md-6 col-sm-4">
											<button
												class="btn btn-sm p-0 shadow-none"
												:class="{ disabled: page === 1 }"
												@click="prevPage"
											>
												<i class="fas fa-angle-left"></i>
											</button>
										</div>
										<div class="col-md-6 col-sm-4">
											<button
												class="btn btn-sm p-0 shadow-none"
												:class="{
													disabled: page === Math.ceil(total / per_page),
												}"
												@click="nextPage"
											>
												<i class="fas fa-angle-right"></i>
											</button>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios';

import Preloader from './Preloader.vue';

export default {
	name: 'Todo',
	data() {
		return {
			todos: [],
			page: 1,
			per_page: 10,
			total: 0,
			loading: false,
		};
	},
	components: {
		Preloader,
	},
	methods: {
		fetchTodos() {
			this.loading = true;
			axios
				.get(`https://jsonplaceholder.typicode.com/todos`)
				.then((response) => {
					let { data } = response;
					this.todos = data.slice(
						(this.page - 1) * this.per_page,
						this.page * this.per_page
					);
					this.total = response.data.length;

					this.loading = false;
				})
				.catch((error) => {
					console.log(error);
				});
		},
		nextPage() {
			this.page++;
			this.fetchTodos();
		},
		prevPage() {
			this.page--;
			this.fetchTodos();
		},
		changePageSize(e) {
			this.per_page = +e.target.value;
			this.page = 1;
			this.fetchTodos();
		},
	},
	mounted() {
		this.fetchTodos();
	},
};
</script>

<style scoped>
.table-responsive {
	max-height: 400px;
}
</style>
