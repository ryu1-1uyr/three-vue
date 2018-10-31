<template>
  <div>

    <div class="controller">

      <input type="button" v-on:click="change_x_y"　value="swhich X <=> Y">
      <input type="button" v-on:click="move">
      <div v-if="test">now Y</div>
      <div v-else="test">now X</div> <br>
      <input v-if="test"   v-model.number="speed" type="number" placeholder="x rotate speed">
      <input v-else="test" v-model.number="speed" type="number" placeholder="y rotate speed">
      <input v-model="color" >
      x:<input v-model="x_scale" type="number" >
      y:<input v-model="y_scale" type="number" >
      z:<input v-model="z_scale" type="number" >

    </div>

    <div class="stage">
      <div ref="stage" v-on:click="move"></div>
    </div>

  </div>
</template>

<script>


  import * as THREE from 'three';

  const rotate = (speed) => {
    if (speed === '') {
      return 0
    } else {
      return speed
    }
  };

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
      light.position.set(1, -10, 5);

      const light2 = new THREE.DirectionalLight(0xffffff);
      light2.position.set(2, 8, 10);

      // === model ===
      const geometry = new THREE.BoxGeometry (1, 1, 1);
      const material = new THREE.MeshStandardMaterial ({ color: 0x123456 });
      const cube = new THREE.Mesh (geometry, material);


      return {
        scene: scene,
        renderer: renderer,
        camera: camera,
        light: light,
        light2: light2,
        cube: cube,

        speed: 0.03,
        // tmp_speed : 0,
        color: 0xffffff,
        x_scale: 1,
        y_scale: 1,
        z_scale: 1,

        test: false  ,
      }
    },

    created () {
      // === sceneにmodel,light, cameraを追加 ===
      this.scene.add( this.camera );
      this.scene.add( this.light);
      this.scene.add( this.light2);
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

         // computed.rotate
         //trueならxのスピードを変更し、yのスピードを保持し、falseならその逆の動作を行う関数がほしい

        if (this.test) {
          this.cube.rotation.x += rotate(this.speed);
        } else {
          this.cube.rotation.y += rotate(this.speed);
        }


        this.cube.scale.x = this.x_scale;
        this.cube.scale.y = this.y_scale;
        this.cube.scale.z = this.z_scale;
        // console.log(this.cube.position)

        this.cube.material.color.setHex(this.color);

        this.renderer.render(this.scene, this.camera);
      },
      change_x_y  () {
        if(this.test){
          this.test = false ;
        }else {
          this.test = true ;
        }
      },
      move (event) {
        console.log(event.screenX,event.screenY)
      }

    },


  }
</script>

<style></style>
