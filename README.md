<!DOCTYPE html>
<html>
<head>
  <title>Terror 3D</title>
  <style>
    body { margin: 0; overflow: hidden; }
  </style>
</head>
<body>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<script>
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x000000);

const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// Luz
const light = new THREE.PointLight(0xffffff, 1, 100);
light.position.set(0, 5, 5);
scene.add(light);

// Chão
const floorGeometry = new THREE.PlaneGeometry(50, 50);
const floorMaterial = new THREE.MeshStandardMaterial({ color: 0x222222 });
const floor = new THREE.Mesh(floorGeometry, floorMaterial);
floor.rotation.x = -Math.PI / 2;
scene.add(floor);

// Player (cubo branco)
const playerGeometry = new THREE.BoxGeometry(1, 2, 1);
const playerMaterial = new THREE.MeshStandardMaterial({ color: 0xffffff });
const player = new THREE.Mesh(playerGeometry, playerMaterial);
player.position.y = 1;
scene.add(player);

// Killer (cubo vermelho)
const killerGeometry = new THREE.BoxGeometry(1, 2, 1);
const killerMaterial = new THREE.MeshStandardMaterial({ color: 0xff0000 });
const killer = new THREE.Mesh(killerGeometry, killerMaterial);
killer.position.set(5, 1, 5);
scene.add(killer);

camera.position.set(0, 5, 10);

let keys = {};

document.addEventListener("keydown", (e) => keys[e.key] = true);
document.addEventListener("keyup", (e) => keys[e.key] = false);

function animate() {
  requestAnimationFrame(animate);

  // Movimento player
  if (keys["w"]) player.position.z -= 0.1;
  if (keys["s"]) player.position.z += 0.1;
  if (keys["a"]) player.position.x -= 0.1;
  if (keys["d"]) player.position.x += 0.1;

  // IA simples do killer
  let dx = player.position.x - killer.position.x;
  let dz = player.position.z - killer.position.z;

  killer.position.x += dx * 0.01;
  killer.position.z += dz * 0.01;

  camera.position.x = player.position.x;
  camera.position.z = player.position.z + 10;
  camera.lookAt(player.position);

  renderer.render(scene, camera);
}

animate();
</script>

</body>
</html>