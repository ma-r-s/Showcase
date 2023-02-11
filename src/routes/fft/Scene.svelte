<script>
	import { OrbitControls, T, useFrame, Pass } from '@threlte/core';
	import { degToRad } from 'three/src/math/MathUtils';
	import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass';
	import { PlaneGeometry, Vector2 } from 'three';
	let t = 0;
	let mesh;
	let geometry;
	let vertices;

	let onChange = () => {
		if (geometry != undefined) {
			geometry = new PlaneGeometry(10, 10, 100, 50);
			vertices = geometry.getAttribute('position').array;

			for (let i = 0; i < vertices.length; i += 3) {
				const x = vertices[i];
				const y = vertices[i + 1];
				vertices[i + 2] = ((3 * Math.sin(x + 2 * t)) / (x - 6) + Math.sin(3 * y + t)) / (y - 6);
			}
			// geometry.computeVertexNormals();
		}
	};

	useFrame((_, delta) => {
		t += 2 * delta;
		onChange();
	});
</script>

<T.PerspectiveCamera position={[-5, 4, -10]} makeDefault>
	<OrbitControls maxPolarAngle={degToRad(90)} />
</T.PerspectiveCamera>
<T.AmbientLight intensity={0.7} />
<T.Mesh position.y={0.5} receiveShadow rotation={[degToRad(-90), 0, 0]} bind:ref={mesh}>
	<T.PlaneGeometry args={[10, 10, 100, 100]} bind:ref={geometry} />
	<T.MeshBasicMaterial color="#999999" wireframe={true} />
</T.Mesh>
<!-- <Pass pass={new UnrealBloomPass(new Vector2(256, 256), 1, 0.1, 0.2)} /> -->
