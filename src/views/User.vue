<template>
	<div class="container">
		<h2 v-if="created">Created...</h2>
		<h3>User Info</h3>
		<form action="javascript:" @submit.prevent="sendUserData">
			<div class="form-field">
				<input
					type="text"
					required
					placeholder="Name..."
					name="name"
					v-model="userForm.name"
				/>
			</div>
			<div class="form-field">
				<input
					type="email"
					placeholder="Email..."
					name="email"
					required
					v-model="userForm.email"
				/>
			</div>
			<div class="form-field" data-tip="PLease enter numbers only">
				<input
					type="tel"
					required
					placeholder="Phone.."
					pattern="[0-9]+"
					name="phone"
					v-model="userForm.phone"
				/>
			</div>
			<div class="image-field">
				<label for="image">Upload Image</label>:
				<input
					@change="imageSelected"
					required
					type="file"
					name="image"
					id="image"
				/>
			</div>
			<div v-if="displayImage" class="image-preview">
				<img :src="displayImage" height="250" width="250" alt="userImage" />
			</div>
			<div class="submit-btn">
				<button class="btn" type="submit">Send Data</button>
			</div>
		</form>
	</div>
</template>

<script>
	export default {
		data: () => {
			return {
				userForm: {
					name: '',
					email: '',
					phone: '',
					image: {}
				},
				displayImage: null,
				userId: '',
				created: false
			};
		},
		methods: {
			imageSelected(e) {
				this.userForm.image = e.target.files[0];
				this.displayImage = URL.createObjectURL(this.userForm.image);
			},
			sendUserData() {
				fetch('https://jsonplaceholder.typicode.com/users', {
					method: 'POST',
					body: this.getFormData(this.userForm)
				})
					.then((res) => res.json())
					.then((json) => {
						this.created = true;
						this.updateUser(json.id);
					});
			},
			updateUser(userId) {
				this.userID = userId;
				this.tweakUserData();
				fetch(`https://jsonplaceholder.typicode.com/users/1`, {
					method: 'PUT',
					body: this.getFormData({ ...this.userForm, id: userId })
				}).then(() => alert(`User created and Updated with id ${userId}`));
			},
			getFormData(data) {
				const formData = new FormData();
				Object.keys(data).forEach((key) => {
					if (data[key]) {
						formData.append(key, data[key]);
					}
				});

				return formData;
			},
			tweakUserData() {
				this.userForm.name = 'John Doe';
				this.userForm.email = 'John@doe.com';
				this.userForm.phone = '01134567889';
				this.userForm.image = null;
			}
		}
	};
</script>

<style lang="scss" scoped>
	.container {
		width: 1174px;
		margin: 0 auto;

		.form-field {
			display: inline-flex;
			width: 35%;
			padding: 1.5rem;

			input {
				padding: 8px;
				width: 100%;
				height: 25px;
			}
		}

		.image-preview {
			margin: 2rem;
		}

		.submit-btn {
			margin-top: 2rem;

			.btn {
				padding: 20px;
				background-color: #3498db;
				border: none;
				border-radius: 20%;
				box-shadow: 4px 6px 3px #bdc3c7;
				color: #f9f9f9;
				font-weight: bold;
				cursor: pointer;
			}
		}
	}
</style>
