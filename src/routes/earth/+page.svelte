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
		let rot = 0;
		// Renderer
		const renderer = new THREE.WebGLRenderer({ antialias: true });
		renderer.setSize(window.innerWidth, window.innerHeight);
		container.appendChild(renderer.domElement);
		// Geometry
		const geometry = new THREE.SphereGeometry(300, 30, 30);
		const material = new THREE.MeshStandardMaterial({
			map: new THREE.TextureLoader().load('/images/earth.jpg'),
			side: THREE.DoubleSide
		});
		const mesh = new THREE.Mesh(geometry, material);
		scene.add(mesh);
		// light
		const directionalLight = new THREE.DirectionalLight(0xffffff);
		directionalLight.position.set(1, 1, 1);
		scene.add(directionalLight);
		// stars
		createStarField();
		function createStarField() {
			// 頂点情報
			const vertices = Array.from({ length: 1000 * 3 }, () => 1000 * 3 * (Math.random() - 0.5));
			// 形状データ
			const geometry = new THREE.BufferGeometry();
			geometry.setAttribute('position', new THREE.Float32BufferAttribute(vertices, 3));
			// マテリアル作成
			const material = new THREE.PointsMaterial({
				size: 10,
				color: 0xffffff
			});
			// 物体作成
			const mesh = new THREE.Points(geometry, material);
			scene.add(mesh);
		}
		// Animation
		function animate() {
			// 自転
			mesh.rotation.y += 0.005;
			// カメラ
			rot += 0.5;
			const radian = (rot * Math.PI) / 180;
			camera.position.x = 1500 * Math.sin(radian);
			camera.position.z = 1500 * Math.cos(radian);
			// 原点方向を見つめる
			camera.lookAt(new THREE.Vector3(0, 0, 0));
			renderer.render(scene, camera);
			requestAnimationFrame(animate);
		}
		animate();
	});
</script>

<div bind:this={container} />
