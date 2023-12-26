<template>
  <div>
    <div class="z-30">
      <h1>JUST 3D</h1>
    </div>
    <canvas class="webgl z-20"></canvas>
  </div>
</template>

<script setup>
import * as THREE from "three";
import * as dat from 'dat.gui'
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls.js'
import gsap from 'gsap'

console.log(THREE)

/**
 * Sizes
 */
const sizes = {}
sizes.width = window.innerWidth
sizes.height = window.innerHeight

window.addEventListener('resize', () =>
{
    // Save sizes
    sizes.width = window.innerWidth
    sizes.height = window.innerHeight

    // Update camera
    camera.aspect = sizes.width / sizes.height
    camera.updateProjectionMatrix()

    // Update renderer
    renderer.setSize(sizes.width, sizes.height)
})

/**
 * Environnements
 */
// Scene
const scene = new THREE.Scene()


// Debug
const gui = new dat.GUI()

const parameters = {
    color: 0xff0000,
    spin: () => {
        gsap.to(mesh.rotation, {duration: 1, y: mesh.rotation.y + Math.PI * 2, delay: .5})
    }
}


gui
    .addColor(parameters, 'color')
    .onChange(() => {
        material.color.set(parameters.color)
    })

gui
    .add(parameters, 'spin')



//Red Cube
// const geometry = new THREE.BoxGeometry(1, 1, 1, 2, 2, 2)

const geometry = new THREE.BoxGeometry(1, 1, 1, 2, 2, 2)


const material = new THREE.MeshBasicMaterial({
    color: parameters.color,
})
const mesh = new THREE.Mesh(geometry, material)

scene.add(mesh)


// Camera
const camera = new THREE.PerspectiveCamera(60, sizes.width / sizes.height, 0.1, 100)

camera.position.z = 3
camera.lookAt(mesh.position)
scene.add(camera)

// Controls
const canvas = document.querySelector('.webgl')

const controls = new OrbitControls(camera, canvas)

controls.enableDamping = true

// Debug tools


// Adding the tweaks
// gui.add(mesh.position, 'y', -3, 3, 0.01)
// gui.add(mesh.position, 'x', -3, 3, 0.01)
// gui.add(mesh.position, 'z', -3, 3, 0.01)

gui
    .add(mesh.position, 'y')
    .min(-3)
    .max(3)
    .step(0.01)
    .name('y position')

gui
    .add(mesh ,'visible')

gui
    .add(material ,'wireframe')

// Renderer
const renderer = new THREE.WebGLRenderer({
    canvas: document.querySelector('.webgl')
})
renderer.setPixelRatio(window.devicePixelRatio)
renderer.setSize(sizes.width, sizes.height)

renderer.render(scene,camera)


// Clock
const clock = new THREE.Clock()

// Animation

/**
 * Loop
 */
const loop = () =>
{
    // Clock
    const elapsedTime = clock.getElapsedTime()

    // Update

    // Update controls
    controls.update()

    // Render
    renderer.render(scene, camera)

    // Keep looping
    window.requestAnimationFrame(loop)
}
loop()

</script>
