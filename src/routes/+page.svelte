<script>
	import { Canvas, OrbitControls, T, Pass } from '@threlte/core';
	import { degToRad } from 'three/src/math/MathUtils';
	import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass';
	import { Vector2 } from 'three';
</script>

<div class="bg-black min-h-screen flex flex-col items-center justify-center overflow-hidden">
	<h1 class="text-3xl text-white font-bold m-5">Tailwind + Threlte</h1>

	<div class="w-full grow">
		<Canvas>
			<T.AmbientLight />
			<T.PerspectiveCamera makeDefault position={[20, 0, 10]}>
				<OrbitControls maxPolarAngle={degToRad(90)} enableZoom={false} autoRotate />
			</T.PerspectiveCamera><T.Mesh position.z={-4}>
				<T.TorusGeometry args={[3, 0.2, 256, 32]} />
				<T.MeshPhysicalMaterial color="#FF1000" />
			</T.Mesh>
			<T.Mesh position.z={-2}>
				<T.TorusGeometry args={[4, 0.2, 256, 32]} />
				<T.MeshPhysicalMaterial color="#FF00FF" />
			</T.Mesh>
			<T.Mesh>
				<T.TorusGeometry args={[5, 0.2, 256, 32]} />
				<T.MeshPhysicalMaterial color="#FF0000" />
			</T.Mesh>
			<T.Mesh position.z={2}>
				<T.TorusGeometry args={[4, 0.2, 256, 32]} />
				<T.MeshPhysicalMaterial color="#FF00FF" />
			</T.Mesh>
			<T.Mesh position.z={4}>
				<T.TorusGeometry args={[3, 0.2, 256, 32]} />
				<T.MeshPhysicalMaterial color="#FF1000" />
			</T.Mesh>
			<Pass pass={new UnrealBloomPass(new Vector2(256, 256), 0.3, 5, 0.0)} />
		</Canvas>
	</div>
</div>
