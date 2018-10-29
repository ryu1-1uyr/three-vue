<template>
  <div>

    <div class="controller">
      <!-- .numberをつけておくことで、valueを自動的に数値に変換してくれる -->
      <input v-model.number="speed" type="number">
      <input v-model="color" >
      x:<input v-model="x_scale" type="number" >
      y:<input v-model="y_scale" type="number" >
      z:<input v-model="z_scale" type="number" >
    </div>

    <div class="stage">
      <div ref="stage"></div>
    </div>

  </div>
</template>

<script>
  import * as THREE from 'three';

  export default {

    name: 'sample',

    data () {
      // === scene ===
      const scene = new THREE.Scene ();

      // === renderer ===
      const renderer = new THREE.WebGLRenderer ();
      renderer.setSize( window.innerWidth, window.innerHeight );

      // === camera ===
      const camera = new THREE.PerspectiveCamera (75, window.innerWidth / window.innerHeight, 0.1, 1000);
      camera.position.z = 5;

      // === light ===
      const light = new THREE.DirectionalLight(0xffffff);
      light.position.set(0, 0, 10);

      // === model ===
      const geometry = new THREE.BoxGeometry (1, 1, 1);
      const material = new THREE.MeshStandardMaterial ({ color: 0x123456 });
      const cube = new THREE.Mesh (geometry, material);


      return {
        scene: scene,
        renderer: renderer,
        camera: camera,
        light: light,
        cube: cube,

        speed: 0.05, // データバインディングできるようにする
        color: 0x123456,
        x_scale: 1,
        y_scale: 1,
        z_scale: 1,
      }
    },

    created () {
      // === sceneにmodel,light, cameraを追加 ===
      this.scene.add( this.camera );
      this.scene.add( this.light);
      this.scene.add( this.cube );
    },

    mounted () {
      // === DOMを追加, animate ===
      this.$refs.stage.appendChild(this.renderer.domElement);
      this.animate();
    },

    methods: {

      animate () {
        requestAnimationFrame( this.animate );

        this.cube.rotation.x += this.rotate; // computed.rotate
        this.cube.rotation.y += this.rotate;

        this.cube.scale.x = this.x_scale;
        this.cube.scale.y = this.y_scale;
        this.cube.scale.z = this.z_scale;

        this.cube.material.color.setHex(this.color);

        this.renderer.render(this.scene, this.camera);
      }

    },

    computed: {

          rotate () {
            // inputに何も入力されていないときに''で認識されてしまうため、回避
            if (this.speed === '') {
              return 0;
            } else {
              return this.speed;
            }
          }

      }

  }
</script>

<style></style>
