<script>
	import { onMount } from 'svelte';
	import ChampInput from './ChampInput.svelte';

	let champName = '';
	let champList = [];

	onMount(async () => {
		let latestPatch = await fetchLatestPatch();
		champList = await fetchChampList(latestPatch);
	});

	async function fetchLatestPatch() {
		let response = await fetch('https://raw.githubusercontent.com/CommunityDragon/Data/master/patches.json');
		let json = await response.json();
		return json.patches[json.patches.length-1].name;
	}

	async function fetchChampList(latestPatch) {
		let response = await fetch(`https://ddragon.leagueoflegends.com/cdn/${latestPatch+'.1'}/data/en_US/champion.json`)
		let json = await response.json();
		let newChampList = [];
		for (let champ in json.data) {
			newChampList.push({
				id: json.data[champ].id.toLowerCase(), 
				name: json.data[champ].name
			});
		}
		return newChampList;
	}

	function goToPage() {
		if (champName) {
			window.location.href = `https://lolalytics.com/lol/${champName}/?tier=platinum_plus&mode=aram`;
		}
	}

	function handleKeydown(event) {
		if (event.key === "Enter") {
			goToPage();
		}
	}
</script>

<style>
	.main {
		text-align: center;
	}
	.form {
		display: flex;
		justify-content: center;
		max-width: 600px;
		margin: auto;
	}
	.ChampInput {
		height: 30px;
		text-align: left;
	}
</style>

<svelte:window on:keydown={handleKeydown}/>

<div class="main">
	<h1>Find a Champion's Lolaytics Aram Page</h1>

	<div class="form">
		<div class='ChampInput'>
			<ChampInput bind:value={champName} champList={champList}/>
		</div>
		<div style="width: 0.5em"/>
		<button on:click|preventDefault={goToPage}> 
			Go
		</button>
	</div>
</div>
