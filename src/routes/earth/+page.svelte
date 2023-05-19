<script lang="ts">
	import { onMount } from 'svelte';

	let container: HTMLDivElement;

	onMount(async () => {
		const THREE = await import('three');
		// Scene
		const scene = new THREE.Scene();
		// Camera
		const camera = new THREE.PerspectiveCamera(
			45,
			window.innerWidth / window.innerHeight,
			1,
			10000
		);
		camera.position.set(0, 0, +1000);
		// Renderer
		const renderer = new THREE.WebGLRenderer({ antialias: true });
		renderer.setSize(window.innerWidth, window.innerHeight);
		container.appendChild(renderer.domElement);
		// Geometry
		const geometry = new THREE.SphereGeometry(300, 30, 30);
		const material = new THREE.MeshStandardMaterial({
			map: new THREE.TextureLoader().load('/images/earth.jpg')
		});
		const mesh = new THREE.Mesh(geometry, material);
		scene.add(mesh);
		// light
		const directionalLight = new THREE.AmbientLight(0xffffff);
		directionalLight.position.set(1, 1, 1);
		scene.add(directionalLight);
		// Animation
		function animate() {
			mesh.rotation.y += 0.005;
			renderer.render(scene, camera);
			requestAnimationFrame(animate);
		}
		animate();
	});
</script>

<div bind:this={container} />
