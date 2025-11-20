<script>
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();

	import '$lib/styles.css';
</script>
<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Epilogue:ital,wght@0,100..900;1,100..900&family=Montserrat:ital,wght@0,100..900;1,100..900&family=Space+Grotesk:wght@300..700&display=swap" rel="stylesheet">

	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}
