<script>
	import { Button } from '$lib/components/ui/button';
	import { Canvas } from '@threlte/core';
	import Scene from './Scene.svelte';
	let cubes = 1;
	let miners = 0;
	let minerPrice = 10;
	let mineRate = 1;
	let ratePrice = 10;
	let cubeSide = 5;
	let sidePrice = 10;
	let time = 1000;
	let timePrice = 10;
	const dimTime = () => {
		if (cubes >= timePrice) {
			cubes -= timePrice;
			time *= 0.99;
			timePrice += 10;
		}
	};
	const buyMiner = () => {
		if (cubes >= minerPrice) {
			cubes -= minerPrice;
			miners++;
			minerPrice += 10;
		}
	};
	const buyRate = () => {
		if (cubes >= ratePrice) {
			cubes -= ratePrice;
			mineRate++;
			ratePrice += 10;
		}
	};
	const incSide = () => {
		if (cubes >= sidePrice) {
			cubes -= sidePrice;
			cubeSide++;
			sidePrice += 10;
		}
	};
	const tick = () => {
		setTimeout(() => {
			cubes + miners > cubeSide ** 3 ? (cubes = cubeSide ** 3) : (cubes += miners);
			tick();
		}, time);
	};
	tick();

	const mine = () => {
		cubes + mineRate > cubeSide ** 3 ? (cubes = cubeSide ** 3) : (cubes += mineRate);
	};
</script>

<div class="container h-dvh font-mono">
	<div class="h-full w-full flex-row-reverse pt-8 md:flex">
		<div class="mb-8 aspect-video overflow-hidden rounded-2xl bg-zinc-950 md:aspect-auto md:w-1/2">
			<Canvas>
				<Scene {cubes} {cubeSide} />
			</Canvas>
		</div>
		<div class="md:w-1/2">
			<div class="mb-4">
				<Button on:click={mine}>Mine</Button>
				Cubes = {cubes}
			</div>
			<div class="mb-4">
				<Button on:click={buyMiner} disabled={minerPrice > cubes}>Miner {minerPrice}$</Button>
				Miners = {miners}
			</div>
			<div class="mb-4">
				<Button on:click={buyRate} disabled={ratePrice > cubes}>Mine Rate {ratePrice}$</Button>
				Mine Rate = {mineRate}
			</div>
			<div class="mb-4">
				<Button on:click={incSide} disabled={sidePrice > cubes}>Cube side {sidePrice}$</Button>
				Cube side = {cubeSide}
			</div>
			<div class="mb-4"></div>
			<Button on:click={dimTime} disabled={timePrice > cubes}>Mine Time {timePrice}$</Button>
			Time = {time.toFixed(3)}
		</div>
	</div>
</div>
