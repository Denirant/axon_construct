<script lang="ts">
	import { Loader, AlertTriangle, RefreshCw } from 'lucide-svelte';
    import { onMount } from 'svelte';

	let status = 'loading'; // "loading", "error", "retrying"
	let countdown = 5;

	let statusTitle = 'Connecting to Axon AI';
	let statusMessage = 'Please wait a little bit...';

	let retryTimeout: NodeJS.Timeout;
	let countdownInterval: NodeJS.Timeout;

	function updateStatus(title: string, message: string) {
		statusTitle = title;
		statusMessage = message;
	}

	function attemptConnection() {
		clearTimeout(retryTimeout);
		clearInterval(countdownInterval);
		status = 'loading';
		updateStatus('Connecting to Axon AI', 'Please wait a little bit...');

		setTimeout(() => {
			connectionFailed();
		}, 40000);
	}

	function connectionFailed() {
		status = 'error';
		countdown = 10;
		updateStatus('Connection failed', `Retrying in ${countdown}s...`);

		countdownInterval = setInterval(() => {
			countdown--;
			if (countdown > 0) {
				updateStatus('Connection failed', `Retrying in ${countdown}s...`);
			} else {
				clearInterval(countdownInterval);
				status = 'loading';
				updateStatus('Reconnecting...', 'Attempting to restore connection...');
				retryTimeout = setTimeout(attemptConnection, 5000);
			}
		}, 1000);
	}

	onMount(() => {
		localStorage.setItem('theme', 'light');
		attemptConnection();
	});
</script>

<main class="h-screen w-full text-gray-900 flex flex-col justify-center items-center px-4 sm:px-6 relative">
	<!-- Branding -->
	<div class="absolute top-10 flex justify-center items-center gap-2 mb-8">
		<img src="favicon.png" alt="AxonAI logo" class="w-7 h-7 rounded-2xl p-0.5 border border-gray-200" />
		<span class="text-lg font-medium tracking-tight text-gray-700">AxonAI</span>
	</div>

	<!-- Status -->
	<div class="bg-white px-6 py-8 w-full max-w-md text-center space-y-2">
		{#if status === 'loading'}
			<div class="flex justify-center">
				<Loader class="w-6 h-6 text-neutral-500 animate-spin" />
			</div>
		{:else}
			<div class="flex justify-center">
				<RefreshCw class="w-6 h-6 text-neutral-400 animate-spin" />
			</div>
		{/if}

		<h1 class="text-xl sm:text-2xl font-semibold tracking-tight mt-10">
			{statusTitle}
		</h1>
		<p class="text-sm sm:text-sm text-gray-500 leading-relaxed">
			{statusMessage}
		</p>
	</div>

	<!-- Footer -->
	<p class="absolute bottom-4 text-xs text-gray-400 font-mono tracking-tight">
		Release 0.13.4.b
	</p>
</main>
