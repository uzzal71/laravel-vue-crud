<template>
	<div class="customer-new">
		<form @submit.prevent="add">
			<table class="table">
				<tr>
					<th>Name</th>
					<td>
						<input type="text" class="form-control" v-model="customer.name" placeholder="Customer Name">
					</td>
				</tr>
				<tr>
					<th>E-mail</th>
					<td>
						<input type="text" class="form-control" v-model="customer.email" placeholder="Customer E-mail">
					</td>
				</tr>
				<tr>
					<th>Phone</th>
					<td>
						<input type="text" class="form-control" v-model="customer.phone" placeholder="Customer Phone">
					</td>
				</tr>
				<tr>
					<th>Website</th>
					<td>
						<input type="text" class="form-control" v-model="customer.website" placeholder="Customer Website">
					</td>
				</tr>
				<tr>
					<td>
						<router-link to="/customers" class="btn">Cancel</router-link>
					</td>
					<td class="text-right">
						<input type="submit" value="Create" class="btn btn-primary">
					</td>
				</tr>
			</table>
		</form>
		<div class="errors" v-if="errors">
			<ul>
				<li v-for="(fieldError, fieldName) in errors" :key="fieldName">
					<strong>{{ fieldName }}</strong> {{ fieldError.join('\n') }}
				</li>
			</ul>
		</div>
	</div>
</template>

<script type="text/javascript">

	import validate from 'validate.js';

	export default {
		name: 'new',
		data() {
			return {
				customer: {
					name: '',
					email: '',
					phone: '',
					website: ''
				},
				errors: null
			}
		},
		computed: {
			currentUser() {
				return this.$store.getters.currentUser;
			}
		},
		methods: {
			add() {
				this.errors = null;

				const constraints = this.getConstraints();

				const errors = validate(this.$data.customer, constraints);

				if(errors) {
					this.errors = errors;
					return;
				}

				// send the customer data to the backend api
				axios.post('/api/customers/new', this.$data.customer, {

            })
				.then((response) =>{
					this.$router.push('/customers');
				});
			},
			getConstraints() {
				return {
					name: {
						presence: true,
						length: {
							minimum: 3,
							message: 'Must be at least 3 characters long'
						}
					},
					email: {
						presence: true,
						email: true
					},
					phone: {
						presence: true,
						numericality: true,
						length: {
							minimum: 11,
							message: 'Must be at least 11 digit long'
						}
					},
					website: {
						presence: true,
						url: true
					}
				};
			}
		}
	}
</script>

<style type="text/css">
	.errors {
		background-color: lightcoral;
		border-radius: 2px;
		padding: 21px 0 2px 0;

	}
</style>