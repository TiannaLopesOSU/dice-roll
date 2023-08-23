<template>
  <div ref="container" class="dice-container"></div>
</template>

<script>
import { ref, onMounted } from "vue";
import * as THREE from "three";

export default {
  name: "DiceRolling",
  setup() {
    const container = ref(null);

    onMounted(() => {
      let scene, camera, renderer, dice;

      const init = () => {
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(
          75,
          window.innerWidth / window.innerHeight,
          0.1,
          1000
        );
        renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        container.value.appendChild(renderer.domElement);

        dice = createDice();
        scene.add(dice);

        camera.position.z = 5;

        animate();
      };

      const animate = () => {
        requestAnimationFrame(animate);

        dice.rotation.x += 0.01;
        dice.rotation.y += 0.01;

        renderer.render(scene, camera);
      };

      init();
    });

    const createDot = () => {
      const dotGeometry = new THREE.CircleGeometry(0.05, 32);
      const dotMaterial = new THREE.MeshBasicMaterial({ color: 0x000000 }); // Black color
      return new THREE.Mesh(dotGeometry, dotMaterial);
    };

    const createDice = () => {
      const diceGroup = new THREE.Group();
      const diceGeometry = new THREE.BoxGeometry(1, 1, 1);
      const diceMaterial = new THREE.MeshBasicMaterial({ color: 0xffffff }); // White color
      const diceMesh = new THREE.Mesh(diceGeometry, diceMaterial);
      diceGroup.add(diceMesh);

      // Add dots on the dice faces
      const dotPositions = [
        { x: -0.4, y: 0.4 },
        { x: 0, y: 0.4 },
        { x: 0.4, y: 0.4 },
        { x: -0.4, y: 0 },
        { x: 0, y: 0 },
        { x: 0.4, y: 0 },
        { x: -0.4, y: -0.4 },
        { x: 0, y: -0.4 },
        { x: 0.4, y: -0.4 },
      ];

      for (const { x, y } of dotPositions) {
        const dot = createDot();
        dot.position.set(x, y, 0.51);
        diceGroup.add(dot);
      }

      return diceGroup;
    };

    return {
      container,
    };
  },
};
</script>

<style scoped>
.dice-container {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
