<template>
	<div class="container my-5">
		<div class="row justify-content-center">
			<div class="col-12 col-sm-12 col-md-8 col-lg-6">
				<router-link :to="{name: 'transaction.index'}" class="btn btn-success btn-sm rounded mb-3 px-3">Back</router-link>
				<div class="card rounded shadow">
					<div class="card-header">Create transaction</div>
					<div class="card-body">
						<form @submit.prevent="store()">
							<div class="mb-3">
								<label for="" class="form-label">Title</label>
								<input type="text" class="form-control" v-model="transaction.title">
								<div class="text-danger" v-if="validation.title">{{ validation.title[0] }}</div>
							</div>

							<div class="mb-3">
								<label for="" class="form-label">Amount</label>
								<input type="text" class="form-control" v-model="transaction.amount">
								<div class="text-danger" v-if="validation.amount">{{ validation.amount[0] }}</div>
							</div>

							<div class="mb-3">
								<label for="" class="form-label">Time</label>
								<input type="text" class="form-control" placeholder="yyyy-mm-dd hh:mm:ss" v-model="transaction.time">
								<div class="text-danger" v-if="validation.time">{{ validation.time[0] }}</div>
							</div>

							<div class="mb-3">
								<label for="" class="form-label">Type</label>
								<select class="form-select" v-model="transaction.type">
									<option value="expense">Expense</option>
									<option value="revenue">Revenue</option>
								</select>
								<div class="text-danger" v-if="validation.type">{{ validation.type[0] }}</div>
							</div>
							<button class="btn btn-primary">Submit</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import { reactive, ref } from 'vue'
	import { useRouter } from 'vue-router'
	import axios from 'axios'
	
	export default {
		setup() {
			const validation = ref([])
			const router = useRouter()
			let transaction = reactive({
				title: '',
				amount: '',
				time: '',
				type: ''
			})

			function store() {
				axios.post('https://laravue-api.app/api/transaction/', transaction)
				.then((result) => {
					router.push({ name: 'transaction.index' })
				}).catch((err) => {
					validation.value = err.response.data
				})
			}
			
			return { transaction, validation, router, store }
		}	
	}
</script>