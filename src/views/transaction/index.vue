<template>
	<div class="container my-5">
		<div class="row justify-content-center">
			<div class="col-12 col-sm-12 col-md-10 col-lg-8">
				<router-link :to="{name: 'transaction.create'}" class="btn btn-success btn-sm rounded mb-3 px-3">Add new</router-link>
				<div class="card rounded shadow">
					<div class="card-header">Transaction List</div>
					<div class="card-body">
						<table class="table table-striped table-borderless">
							<thead>
								<tr>
									<th>Title</th>
									<th>Amount</th>
									<th>Type</th>
									<th class="text-center">Action</th>
								</tr>
							</thead>
							<tbody>
								<tr v-for="(transaction, index) in transactions.data" :key="index">
									<td>{{ transaction.title }}</td>
									<td>{{ 'IDR ' + transaction.amount }}</td>
									<td>{{ transaction.type == 'revenue' ? 'Revenue' : 'Expense' }}</td>
									<td class="text-center">
										<div class="btn-group" role="group" aria-label="Action">
											<router-link :to="{
													name: 'transaction.edit', 
													params: { id: transaction.id }
												}" class="btn btn-info btn-sm">Edit</router-link>
											<button class="btn btn-light btn-sm" @click.prevent="destroy(transaction.id, index)">Delete</button>
										</div>
									</td>
								</tr>
							</tbody>
						</table>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import axios from 'axios'
	import {onMounted, ref} from 'vue'

	export default {
		setup() {
			// Dynamic state
			let transactions = ref([])
			
			onMounted(() => {
				// Get data from API endpoint
				axios.get('https://laravue-api.app/api/transaction')
				.then((result) => {
					transactions.value = result.data
				}).catch((err) => {
					console.log(err.response)
				})
			})

			function destroy(id, index) {
				let conf = confirm('Yakin akan menghapus?');
				if (conf) {
					axios.delete(`https://laravue-api.app/api/transaction/${id}`)
					.then((result) => {
						transactions.value.data.splice(index, 1)
					}).catch((err) => {
						console.log(err.response.data)
					})
				}
			}

			return { transactions, destroy }
		}
	}
</script>