<script>


	let wids = [];
	let tabs_by_window = {};
	function loadAll(){
	chrome.tabs.query({},(tabs)=>{
		for(var i = 0, max = tabs.length; i< max; i++ ){
			let wid = tabs[i].windowId;
			if(!wids.includes(wid)) wids.push(wid);
			if (tabs_by_window[wid] == undefined){
				tabs_by_window[wid] = [];
			}
			tabs_by_window[wid].push(tabs[i]);
		}
		wids.sort();
		wids.forEach(wid=>{tabs_by_window[wid].sort((a,b)=>{return a-b;});});
		
	});
	};

</script>
<main>
<h2>BCSM:</h2>
<br/>
<button on:click={loadAll}>
	Load all tabs
</button>
<div>
	<ol>
		{#each wids as wid}
		{wid}
		<ul>
			{#each tabs_by_window[wid] as tab}
			<li>
				{#if tab.selected}
					<span class="active">
						{tab.id} | <a href="{tab.url}">{tab.title}</a>
					</span>
				{:else}
					<span class="inactive">
						{tab.id} | <a href="{tab.url}">{tab.title}</a>
					</span>
				{/if}
			</li>
			{/each}
		</ul>
		{/each}
	</ol>
	
</div>
</main>

<style>
	main {
		min-width: 700px;
 		max-width: 100%;
	}
	.active {
		font-weight: 700;
	}
	.inactive {
		font-weight: 400;
	}
</style>