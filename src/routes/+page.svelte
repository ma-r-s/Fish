<script>
	import { Button } from '$lib/components/ui/button';
	import { Canvas } from '@threlte/core';
	import Scene from './Scene.svelte';
	let gold = 1;
	let miners = 0;
	let minerPrice = 10;
	let mineRate = 1;
	let ratePrice = 10;
	let cubeSide = 5;
	let sidePrice = 10;
	let time = 1000;
	let timePrice = 10;
	const dimTime = () => {
		if (gold >= timePrice) {
			gold -= timePrice;
			time *= 0.9;
			timePrice += 10;
		}
	};
	const buyMiner = () => {
		if (gold >= minerPrice) {
			gold -= minerPrice;
			miners++;
			minerPrice += 10;
		}
	};
	const buyRate = () => {
		if (gold >= ratePrice) {
			gold -= ratePrice;
			mineRate++;
			ratePrice += 10;
		}
	};
	const incSide = () => {
		if (gold >= sidePrice) {
			gold -= sidePrice;
			cubeSide++;
			sidePrice += 10;
		}
	};
	const tick = () => {
		setTimeout(() => {
			gold + miners > cubeSide ** 3 ? (gold = cubeSide ** 3) : (gold += miners);
			tick();
		}, time);
	};
	tick();

	const mine = () => {
		gold + mineRate > cubeSide ** 3 ? (gold = cubeSide ** 3) : (gold += mineRate);
	};
</script>

<div class="container mt-10 font-mono">
	<div class="mb-6 aspect-video overflow-hidden rounded-2xl bg-zinc-950">
		<Canvas>
			<Scene {gold} {miners} {cubeSide} />
		</Canvas>
	</div>
	<div class="mb-4">
		<Button on:click={mine}>Mine</Button>
		Gold = {gold}
	</div>
	<div class="mb-4">
		<Button on:click={buyMiner} disabled={minerPrice > gold}>Miner {minerPrice}$</Button>
		Miners = {miners}
	</div>
	<div class="mb-4">
		<Button on:click={buyRate} disabled={ratePrice > gold}>Mine Rate {ratePrice}$</Button>
		Mine Rate = {mineRate}
	</div>
	<!-- Increase cube side -->
	<div class="mb-4">
		<Button on:click={incSide} disabled={sidePrice > gold}>Cube side {sidePrice}$</Button>
		Cube side = {cubeSide}
	</div>
	<div class="mb-4"></div>
	<Button on:click={dimTime} disabled={timePrice > gold}>Mine Time {timePrice}$</Button>
	Time = {time}
</div>
