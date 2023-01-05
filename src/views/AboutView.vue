<template>
  <canvas id="webgl"></canvas>
</template>

<script>
import * as THREE from 'three'
// import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

/**
 * Base
 */

export default {
  name: 'AboutView',

  mounted() {

    // Cursor
    const cursor = {
        x: 0,
        y: 0
    }
    window.addEventListener('mousemove', (event) => {
        cursor.x = event.clientX / sizes.width - 0.5
        cursor.y = event.clientY / sizes.height - 0.5
    })

    // Canvas
    const canvas = document.querySelector('#webgl')

    // Scene
    const scene = new THREE.Scene()

    let dpr = window.devicePixelRatio

    canvas.width = window.innerWidth * dpr
    canvas.height = window.innerHeight * dpr
    canvas.style.maxWidth = window.innerWidth + "px"
    canvas.style.maxHeight = window.innerHeight + "px"

    /**
     * Test mesh
     */
    // Geometry
    const geometry = new THREE.BoxGeometry(1, 1, 1)

    // Material
    const material = new THREE.MeshBasicMaterial({ color: 0xff0000 })

    // Mesh
    const cube = new THREE.Mesh(geometry, material)
    scene.add(cube)



    /**
     * Sizes
     */
    const sizes = {
        width: window.innerWidth,
        height: window.innerHeight
    }

    window.addEventListener('resize', () =>
    {
        // Update sizes
        sizes.width = window.innerWidth
        sizes.height = window.innerHeight

        // Update camera
        camera.aspect = sizes.width / sizes.height
        camera.updateProjectionMatrix()

        // Update renderer
        renderer.setSize(sizes.width, sizes.height)
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
    })

    /**
     * Camera
     */
    // Base camera
    const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 5)
    camera.position.set(0, 0, 2)
    scene.add(camera)

    // Controls
    // const controls = new OrbitControls(camera, canvas)
    // controls.enableDamping = true

    /**
     * Renderer
     */
    const renderer = new THREE.WebGLRenderer({
        canvas: canvas
    })
    renderer.setSize(sizes.width, sizes.height)
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))


    /**
     * Animate
     */

    const clock = new THREE.Clock()
    const tick = () =>
    {
        let elapsedTime = clock.getElapsedTime()

        // Update objects
        cube.rotation.y = elapsedTime

        // Render
        renderer.render(scene, camera)

        // Call tick again on the next frame
        window.requestAnimationFrame(tick)
    }

    tick()
  }
}
</script>


<style lang="sass" scoped>
#webgl
  width: 100vw
  height: 100vh
</style>