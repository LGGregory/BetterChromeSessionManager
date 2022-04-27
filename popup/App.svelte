<script>
	import {onDestroy, onMount} from "svelte";

	let data = getAllTabs();
	onMount(() => {
		
	});
	
	async function getAllTabs(){
		let [tabs] = await chrome.tabs.query();
		tabs_by_window = {};
		wids = []
		for(var i = 0, max = tabs.length; i< max; i++ ){
			wid = tabs[i].windowId;
			if(!wids.includes(wid)) wids.push(wid);
			if (tabs_by_window[wid] == undefined){
				tabs_by_window[wid] = [];
			}
			tabs_by_window[wid].push(tabs[i]);
		}
		wids.sort();
		wids.foreach((wid)=>{tabs_by_window[i].sort((a,b)=>{return a-b;});});
		return {windows:wids, tabs:tabs_by_window};
	}

</script>
<main>
<h2>BCSM:</h2>
<br/>
<div>
	{#await data}
	loading
	{:then data}
	<ol>
		{#each data.windows as wid}
		{wid}
		<ul>
			{#each data.tabs[wid] as tab}
			{#if tab.selected}
			<span class="active">
				{tab.id} | <a href="{tab.url}">{tab.title}</a>
			</span>
			{:else}
			<span class="inactive">
				{tab.id} | <a href="{tab.url}">{tab.title}</a>
			</span>
			{/if}
			{/each}
		</ul>
		{/each}
	</ol>
	{/await}
</div>
</main>

<style>
	main {
		background-color: blue;
	}
	.active {
		font-weight: 700;
	}
	.inactive {
		font-weight: 400;
	}
</style>