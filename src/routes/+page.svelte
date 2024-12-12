<script>
	import { onMount } from 'svelte';

	let form = {
		firstName: '',
		lastName: '',
		country: '',
		dateOfBirth: '',
		internalTransfers: false,
		maxTransferAmount: ''
	};

	let countries = ['United States', 'Uzbekistan', 'Russian Federation', 'South Korea'].sort();
	let isFormValid = false;

	const validateForm = () => {
		isFormValid = form.firstName && form.lastName && form.country && form.dateOfBirth;
		if (form.internalTransfers) {
			isFormValid = isFormValid && form.maxTransferAmount;
		}
	};

	const handleSave = () => {
		console.log(form);
		alert('Form saved successfully!');
		document.querySelectorAll('input, select').forEach((el) => (el.disabled = true));
	};
</script>

<div class="form-container mt-20">
	<h1 class="title">Editing User Details</h1>

	<form class="form-grid">
		<div class="form-group">
			<label class="label">First Name</label>
			<input type="text" bind:value={form.firstName} class="input" on:input={validateForm} />
		</div>

		<div class="form-group">
			<label class="label">Last Name</label>
			<input type="text" bind:value={form.lastName} class="input" on:input={validateForm} />
		</div>

		<div class="form-group col-span-full">
			<label class="label">Country</label>
			<select bind:value={form.country} class="input" on:change={validateForm}>
				<option value="">Select a country</option>
				{#each countries as country}
					<option value={country}>{country}</option>
				{/each}
			</select>
		</div>

		<div class="form-group col-span-full">
			<label class="label">Date of Birth</label>
			<input type="date" bind:value={form.dateOfBirth} class="input" on:input={validateForm} />
		</div>

		<div class="form-group col-span-full">
			<label class="label">
				<input type="checkbox" bind:checked={form.internalTransfers} on:change={validateForm} />
				Internal Transfers
			</label>
		</div>

		{#if form.internalTransfers}
			<div class="form-group col-span-full">
				<label class="label">Max Transfer Amount (USD)</label>
				<input
					type="text"
					bind:value={form.maxTransferAmount}
					class="input"
					on:input={(e) => {
						form.maxTransferAmount = parseFloat(e.target.value.replace(/,/g, '')) || '';
						validateForm();
					}}
					placeholder="e.g., 20,000.23"
				/>
			</div>
		{/if}

		<div class="form-group col-span-full">
			<button type="button" class="button" disabled={!isFormValid} on:click={handleSave}>
				Save
			</button>
		</div>
	</form>
</div>

<style>
	/* Form container */
	.form-container {
		@apply mx-auto max-w-lg rounded-lg bg-gray-100 p-6 shadow-lg;
	}

	/* Title */
	.title {
		@apply mb-4 text-center text-xl font-bold;
	}

	/* Form layout */
	.form-grid {
		@apply grid grid-cols-1 gap-4 md:grid-cols-2;
	}

	/* Form group (input sections) */
	.form-group {
		@apply mb-4;
	}

	/* Label styling */
	.label {
		@apply mb-1 block font-medium text-gray-700;
	}

	/* Input and select elements */
	.input {
		@apply w-full rounded-lg border bg-white p-2 text-gray-700;
	}

	/* Button styles */
	.button {
		@apply w-full rounded-lg bg-blue-500 p-2 font-semibold text-white transition-all duration-200 ease-in-out;
	}

	/* Button disabled state */
	.button:disabled {
		@apply cursor-not-allowed opacity-70;
	}

	/* Hover and focus effects for inputs and button */
	.button:hover {
		@apply bg-blue-600;
	}

	.input:focus {
		@apply border-blue-500 outline-none;
	}

	/* Checkbox styling */
	input[type='checkbox'] {
		@apply mr-2;
	}

	/* Placeholder text style */
	.input::placeholder {
		@apply text-gray-400;
	}
	/* Success Alert Styles */
	.alert-success {
		@apply animate-fadeIn mt-6 rounded-lg bg-green-500 p-4 text-center text-white shadow-lg;
	}

	/* Keyframes for the fade-in animation */
	@keyframes fadeIn {
		0% {
			opacity: 0;
			transform: translateY(-20px);
		}
		100% {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.animate-fadeIn {
		animation: fadeIn 1s ease-out;
	}
</style>
