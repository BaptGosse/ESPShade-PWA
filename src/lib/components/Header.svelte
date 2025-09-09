<!-- Header.svelte -->
<script lang="ts">
	import { onMount } from 'svelte';
	
	export let currentUser: { email: string } | null = null;
	
	let mobileMenuOpen = false;
	let mounted = false;

	onMount(() => {
		mounted = true;
	});

	function toggleMobileMenu() {
		mobileMenuOpen = !mobileMenuOpen;
	}

	function closeMobileMenu() {
		mobileMenuOpen = false;
	}

	function handleLogout() {
		// Ici vous appellerez votre fonction de logout
		if (confirm('Êtes-vous sûr de vouloir vous déconnecter ?')) {
			console.log('Logout...');
			// window.location.href = '/auth/logout';
		}
	}

	function scanNetwork() {
		console.log('Scan réseau déclenché');
		// Ici vous appellerez votre API de scan
		alert('Scan réseau en cours...');
	}
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
	<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.0/font/bootstrap-icons.css">
</svelte:head>

<header class="header">
	<div class="header-container">
		<!-- Logo -->
		<div class="logo">
			<h1><i class="bi bi-house-door"></i> ESPShade</h1>
			<span class="version">v1.0</span>
		</div>

		<!-- Navigation Desktop -->
		<nav class="nav-desktop">
			<a href="/" class="nav-link">
				<i class="bi bi-speedometer2"></i>
				Tableau de bord
			</a>
			<a href="/devices" class="nav-link">
				<i class="bi bi-sliders"></i>
				Mes Volets
			</a>
			<a href="/schedules" class="nav-link">
				<i class="bi bi-clock"></i>
				Planifications
			</a>
			<a href="/statistics" class="nav-link">
				<i class="bi bi-graph-up"></i>
				Statistiques
			</a>
		</nav>

		<!-- Actions Desktop -->
		<div class="header-actions">
			<button class="btn btn-scan" on:click={scanNetwork}>
				<i class="bi bi-search"></i>
				Scanner
			</button>
			
			{#if currentUser}
				<div class="user-menu">
					<span class="user-info">
						<i class="bi bi-person-circle"></i>
						{currentUser.email}
					</span>
					<button class="btn btn-logout" on:click={handleLogout}>
						<i class="bi bi-door-open"></i>
						Déconnexion
					</button>
				</div>
			{:else}
				<a href="/auth/login" class="btn btn-login">
					<i class="bi bi-key"></i>
					Connexion
				</a>
			{/if}
		</div>

		<!-- Menu Hamburger Mobile -->
		<button 
			class="hamburger {mobileMenuOpen ? 'active' : ''}"
			on:click={toggleMobileMenu}
			aria-label="Menu"
		>
			<span></span>
			<span></span>
			<span></span>
		</button>
	</div>

	<!-- Menu Mobile -->
	{#if mounted}
		<div class="mobile-menu {mobileMenuOpen ? 'open' : ''}">
			<nav class="mobile-nav">
				<div class="mobile-nav-header">
					<h2><i class="bi bi-house-door"></i> ESPShade</h2>
					<button class="close-btn" on:click={closeMobileMenu}>
						<i class="bi bi-x-lg"></i>
					</button>
				</div>

				<div class="mobile-nav-content">
					{#if currentUser}
						<div class="mobile-user-info">
							<span class="user-avatar">
								<i class="bi bi-person-fill"></i>
							</span>
							<span class="user-email">{currentUser.email}</span>
						</div>
					{/if}

					<div class="mobile-nav-links">
						<a href="/" class="mobile-nav-link" on:click={closeMobileMenu}>
							<i class="bi bi-speedometer2"></i>
							Tableau de bord
						</a>
						<a href="/devices" class="mobile-nav-link" on:click={closeMobileMenu}>
							<i class="bi bi-sliders"></i>
							Mes Volets
						</a>
						<a href="/schedules" class="mobile-nav-link" on:click={closeMobileMenu}>
							<i class="bi bi-clock"></i>
							Planifications
						</a>
						<a href="/statistics" class="mobile-nav-link" on:click={closeMobileMenu}>
							<i class="bi bi-graph-up"></i>
							Statistiques
						</a>
					</div>

					<div class="mobile-nav-actions">
						<button class="mobile-btn btn-scan" on:click={() => {scanNetwork(); closeMobileMenu();}}>
							<i class="bi bi-search"></i>
							Scanner Réseau
						</button>
						
						{#if currentUser}
							<button class="mobile-btn btn-logout" on:click={() => {handleLogout(); closeMobileMenu();}}>
								<i class="bi bi-door-open"></i>
								Déconnexion
							</button>
						{:else}
							<a href="/auth/login" class="mobile-btn btn-login" on:click={closeMobileMenu}>
								<i class="bi bi-key"></i>
								Connexion
							</a>
						{/if}
					</div>
				</div>
			</nav>
		</div>
	{/if}
</header>

<style>
	* {
		font-family: 'Poppins', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
	}

	.header {
		background: linear-gradient(135deg, #B8412D 0%, #dc2626 100%);
		backdrop-filter: blur(10px);
		border-bottom: 1px solid rgba(255, 255, 255, 0.2);
		box-shadow: 0 2px 20px rgba(0, 0, 0, 0.1);
	}

	.header-container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 20px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		height: 70px;
	}

	.logo h1 {
		color: white;
		font-size: 24px;
		font-weight: 700;
		margin: 0;
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.logo h1 i {
		font-size: 28px;
	}

	.version {
		background: rgba(255, 255, 255, 0.2);
		color: white;
		font-size: 12px;
		padding: 2px 8px;
		border-radius: 12px;
		font-weight: 500;
	}

	/* Navigation Desktop */
	.nav-desktop {
		display: flex;
		gap: 30px;
		align-items: center;
	}

	.nav-link {
		color: rgba(255, 255, 255, 0.9);
		text-decoration: none;
		display: flex;
		align-items: center;
		gap: 8px;
		padding: 8px 12px;
		border-radius: 8px;
		transition: all 0.3s ease;
		font-weight: 500;
	}

	.nav-link:hover {
		background: rgba(255, 255, 255, 0.1);
		color: white;
		transform: translateY(-1px);
	}

	.nav-link i {
		font-size: 16px;
	}

	/* Actions Desktop */
	.header-actions {
		display: flex;
		align-items: center;
		gap: 15px;
	}

	.btn {
		padding: 10px 16px;
		border: none;
		border-radius: 8px;
		cursor: pointer;
		font-weight: 500;
		transition: all 0.3s ease;
		display: flex;
		align-items: center;
		gap: 6px;
		text-decoration: none;
		font-size: 14px;
	}

	.btn i {
		font-size: 14px;
	}

	.btn-scan {
		background: rgba(184, 65, 45, 0.9);
		color: white;
	}

	.btn-scan:hover {
		background: #B8412D;
		transform: translateY(-2px);
	}

	.user-menu {
		display: flex;
		align-items: center;
		gap: 12px;
	}

	.user-info {
		color: white;
		font-size: 14px;
		display: flex;
		align-items: center;
		gap: 6px;
	}

	.user-info i {
		font-size: 18px;
		background: rgba(255, 255, 255, 0.2);
		padding: 4px;
		border-radius: 50%;
	}

	.btn-logout {
		background: rgba(220, 53, 69, 0.9);
		color: white;
	}

	.btn-logout:hover {
		background: #dc3545;
		transform: translateY(-2px);
	}

	.btn-login {
		background: rgba(184, 65, 45, 0.9);
		color: white;
	}

	.btn-login:hover {
		background: #B8412D;
		transform: translateY(-2px);
	}

	/* Hamburger Menu */
	.hamburger {
		display: none;
		flex-direction: column;
		background: none;
		border: none;
		cursor: pointer;
		padding: 8px;
		gap: 4px;
	}

	.hamburger span {
		width: 25px;
		height: 3px;
		background: white;
		border-radius: 2px;
		transition: all 0.3s ease;
	}

	.hamburger.active span:nth-child(1) {
		transform: rotate(45deg) translate(6px, 6px);
	}

	.hamburger.active span:nth-child(2) {
		opacity: 0;
	}

	.hamburger.active span:nth-child(3) {
		transform: rotate(-45deg) translate(6px, -6px);
	}

	/* Menu Mobile */
	.mobile-menu {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: 1000;
		transform: translateX(-100%);
		transition: transform 0.3s ease;
	}

	.mobile-menu.open {
		transform: translateX(0);
	}

	.mobile-menu-overlay {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0, 0, 0, 0.5);
		backdrop-filter: blur(5px);
	}

	.mobile-nav {
		position: absolute;
		left: 0;
		top: 0;
		bottom: 0;
		width: 300px;
		max-width: 85vw;
		background: white;
		box-shadow: 5px 0 20px rgba(0, 0, 0, 0.1);
		display: flex;
		flex-direction: column;
	}

	.mobile-nav-header {
		background: linear-gradient(135deg, #B8412D 0%, #dc2626 100%);
		color: white;
		padding: 20px;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.mobile-nav-header h2 {
		margin: 0;
		font-size: 20px;
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.mobile-nav-header h2 i {
		font-size: 24px;
	}

	.close-btn {
		background: none;
		border: none;
		color: white;
		font-size: 20px;
		cursor: pointer;
		padding: 4px;
		border-radius: 4px;
		transition: background 0.3s ease;
	}

	.close-btn:hover {
		background: rgba(255, 255, 255, 0.1);
	}

	.mobile-nav-content {
		flex: 1;
		padding: 20px;
		display: flex;
		flex-direction: column;
		gap: 30px;
	}

	.mobile-user-info {
		display: flex;
		align-items: center;
		gap: 12px;
		padding: 15px;
		background: #f5f5f5;
		border-radius: 8px;
	}

	.user-avatar {
		width: 40px;
		height: 40px;
		background: #B8412D;
		color: white;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 18px;
	}

	.user-email {
		font-weight: 500;
		color: #333;
	}

	.mobile-nav-links {
		display: flex;
		flex-direction: column;
		gap: 5px;
	}

	.mobile-nav-link {
		color: #333;
		text-decoration: none;
		padding: 15px;
		border-radius: 8px;
		display: flex;
		align-items: center;
		gap: 12px;
		transition: background 0.3s ease;
		font-weight: 500;
	}

	.mobile-nav-link:hover {
		background: #f5f5f5;
	}

	.mobile-nav-link i {
		font-size: 18px;
		width: 20px;
		color: #B8412D;
	}

	.mobile-nav-actions {
		display: flex;
		flex-direction: column;
		gap: 12px;
		margin-top: auto;
	}

	.mobile-btn {
		padding: 15px;
		border: none;
		border-radius: 8px;
		cursor: pointer;
		font-weight: 500;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 8px;
		text-decoration: none;
		transition: all 0.3s ease;
	}

	.mobile-btn i {
		font-size: 16px;
	}

	.mobile-btn.btn-scan {
		background: #B8412D;
		color: white;
	}

	.mobile-btn.btn-logout {
		background: #dc3545;
		color: white;
	}

	.mobile-btn.btn-login {
		background: #B8412D;
		color: white;
	}

	.mobile-btn:hover {
		opacity: 0.9;
		transform: translateY(-1px);
	}

	/* Responsive */
	@media (max-width: 768px) {
		.nav-desktop,
		.header-actions {
			display: none;
		}

		.hamburger {
			display: flex;
		}

		.header-container {
			padding: 0 15px;
		}

		.logo h1 {
			font-size: 20px;
		}

		.logo h1 i {
			font-size: 24px;
		}
	}

	@media (max-width: 480px) {
		.mobile-nav {
			width: 100vw;
		}
	}
</style>