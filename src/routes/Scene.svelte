<script>
	import { T, useTask } from '@threlte/core';
	import { OrbitControls } from '@threlte/extras';
	export let cubes = 10;
	export let miners = 10;
	export let cubeSide = 10;
	let rotation = 0;
	useTask((delta) => {
		rotation += delta;
	});
</script>

<T.PerspectiveCamera
	makeDefault
	position={[15, 15, 15]}
	on:create={({ ref }) => {
		ref.lookAt(0, 100, 0);
	}}
>
	<OrbitControls autoRotate />
</T.PerspectiveCamera>
<T.Group position={[-cubeSide + 1, -cubeSide + 4, -cubeSide + 1]}>
	{#each Array(cubes) as _, i}
		<T.Mesh
			rotation.y={i + rotation}
			position={[
				((i % cubeSide) * 2) % cubeSide ** 2,
				Math.floor(i / cubeSide ** 2) * 2,
				(Math.floor(i / cubeSide) * 2) % (cubeSide * 2)
			]}
		>
			<T.BoxGeometry />
			<T.MeshNormalMaterial />
		</T.Mesh>
	{/each}
</T.Group>
