<script lang="ts">
	import { onMount } from 'svelte';
	import { page } from '$app/state';
	let { children } = $props();

	onMount(() => {
		const setTheme = () => {
			if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
				document.documentElement.style.colorScheme = 'dark';
			} else {
				document.documentElement.style.colorScheme = 'light';
			}
		};

		// Set theme on initial load
		setTheme();

		// Watch for changes in the media query's value
		window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', setTheme);

		return () => {
			// Cleanup listener on component destroy
			window.matchMedia('(prefers-color-scheme: dark)').removeEventListener('change', setTheme);
		};
	});
</script>

<header>
	<nav>
		<div class="navbar-container container">
			<input type="checkbox">
			<div class="hamburger-lines">
				<span></span>
				<span></span>
				<span></span>
			</div>
			<ul class="menu-items">
				<li>
					<a aria-current={page.route.id === '/' ? 'page' : undefined} href="/">home</a>
				</li>
				<li>
					<a
						aria-current={page.route.id?.startsWith('/container') ? 'page' : undefined}
						href="/container"
					>
						containers
					</a>
				</li>
			</ul>
			<a class="logo" href="/">admin.danteb.com</a>
		</div>
	</nav>
</header>

<main>
	{@render children()}
</main>

<footer>
	<p id="copyright">&copy; 2025 Dante Barbieri. All rights reserved.</p>
</footer>

<style>
	header {
		position: relative;
		z-index: 2;
		width: 100%;
		padding-bottom: 4vw;
		pointer-events: auto;
	}

	@media only screen and (pointer: coarse) and (max-width: 1024px), screen and (max-width: 800px) {
		header {
			padding: 6vw;
			margin-bottom: 1.25rem;
		}
	}

	a:not([aria-current='page']):hover {
		border-bottom: 1px solid currentColor;
		transition: border-bottom 0.2s ease-in-out;
	}

	a[aria-current='page'] {
		border-bottom: 1px solid currentColor;
	}

	nav input[type='checkbox'],
	nav .hamburger-lines {
		display: none;
	}

	.container {
		max-width: 1200px;
		width: 90%;
		margin: auto;
	}

	nav {
		box-shadow: 0px 5px 10px 0px var(--background-100);
		position: fixed;
		width: 100%;
		background: var(--background-50);
		color: var(--text);
		opacity: 0.85;
		z-index: 100;
	}

	.navbar-container {
		display: flex;
		justify-content: space-between;
		height: 64px;
		align-items: center;
	}

	.menu-items {
		order: 2;
		display: flex;
	}

	.logo {
		order: 1;
		font-size: 2.3rem;
		color: var(--text);
	}

	.menu-items li {
		list-style: none;
		margin-left: 1.5rem;
		font-size: 1.3rem;
	}

	nav .menu-items a {
		color: var(--secondary-700);
		text-decoration: none;
		font-weight: 500;
		transition: color 0.3s ease-in-out;
	}

	nav .menu-items a:hover {
		color: var(--secondary-500);
	}

	@media (max-width: 768px) {
		nav {
			opacity: 0.95;
		}

		.navbar-container input[type='checkbox'],
		.navbar-container .hamburger-lines {
			display: block;
		}

		.navbar-container {
			display: block;
			position: relative;
			height: 64px;
		}

		.navbar-container input[type='checkbox'] {
			position: absolute;
			display: block;
			height: 32px;
			width: 30px;
			top: 20px;
			left: 20px;
			z-index: 5;
			opacity: 0;
			cursor: pointer;
		}

		.navbar-container .hamburger-lines {
			display: block;
			height: 28px;
			width: 35px;
			position: absolute;
			top: 20px;
			left: 20px;
			z-index: 2;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
		}

		.navbar-container .hamburger-lines span {
			display: block;
			height: 4px;
			width: 100%;
			border-radius: 10px;
			background: var(--text);
		}

		.navbar-container .hamburger-lines span:nth-child(1) {
			transform-origin: 0% 0%;
			transition: transform 0.3s ease-in-out;
		}

		.navbar-container .hamburger-lines span:nth-child(2) {
			transition: transform 0.2s ease-in-out;
		}

		.navbar-container .hamburger-lines span:nth-child(3) {
			transform-origin: 0% 100%;
			transition: transform 0.3s ease-in-out;
		}

		nav .menu-items {
			padding-top: 100px;
			background: var(--background-50);
			height: 100vh;
			max-width: 300px;
			transform: translate(-150%);
			display: flex;
			flex-direction: column;
			margin-left: -40px;
			padding-left: 40px;
			transition: transform 0.5s ease-in-out;
			box-shadow: 5px 0px 10px 0px var(--background-100);
			overflow: scroll;
		}

		nav .menu-items li {
			margin-bottom: 1.8rem;
			font-size: 1.1rem;
			font-weight: 500;
		}

		.logo {
			position: absolute;
			top: 0;
			right: 15px;
			font-size: 2.5rem;
		}

		.navbar-container input[type='checkbox']:checked ~ .menu-items {
			transform: translateX(0);
		}

		.navbar-container input[type='checkbox']:checked ~ .hamburger-lines span:nth-child(1) {
			transform: rotate(45deg);
		}

		.navbar-container input[type='checkbox']:checked ~ .hamburger-lines span:nth-child(2) {
			transform: scaleY(0);
		}

		.navbar-container input[type='checkbox']:checked ~ .hamburger-lines span:nth-child(3) {
			transform: rotate(-45deg);
		}
	}

	@media (max-width: 500px) {
		.navbar-container input[type='checkbox']:checked ~ .logo {
			display: none;
		}
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		width: 100%;
		margin: 0 auto;
		padding-left: 6vw;
		padding-right: 6vw;
	}

	footer {
		margin: 0 auto;
		margin-top: auto;
		align-items: flex-start;
		display: flex;
		flex-direction: column;
		padding: 3.3vmax 0;
		justify-content: center;
		width: 100%;
		padding: 0 6vw;
	}
</style>
