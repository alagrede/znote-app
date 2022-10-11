# ThreeGlobe World map

**Install**
- three - `https://unpkg.com/three@0.145.0/build/three.js`
- threeControls - `https://unpkg.com/three/examples/js/controls/TrackballControls.js`
- threeGlobe - `https://unpkg.com/three-globe@2.24.8/dist/three-globe.min.js`

```js scripts=three,threeControls,threeGlobe
//hide
//  Gen random data
const N = 300;
const gData = [...Array(N).keys()].map(() => ({
    lat: (Math.random() - 0.5) * 180,
    lng: (Math.random() - 0.5) * 360,
    size: Math.random() / 3,
    color: ['red', 'white', 'blue', 'green'][Math.round(Math.random() * 3)]
}));

const Globe = new ThreeGlobe()
    .globeImageUrl('https://unpkg.com/three-globe/example/img/earth-dark.jpg')
    .bumpImageUrl('https://unpkg.com/three-globe/example/img/earth-topology.png')
    .pointsData(gData)
    .pointAltitude('size')
    .pointColor('color');

setTimeout(() => {
    gData.forEach(d => d.size = Math.random());
    Globe.pointsData(gData);
}, 4000);

// Setup renderer
const renderer = new THREE.WebGLRenderer();
renderer.setSize(500, 500);
htmlEl.appendChild(renderer.domElement);

// Setup scene
const scene = new THREE.Scene();
scene.add(Globe);
scene.add(new THREE.AmbientLight(0xbbbbbb));
scene.add(new THREE.DirectionalLight(0xffffff, 0.6));

// Setup camera
const camera = new THREE.PerspectiveCamera();
camera.aspect = 500/500;
camera.updateProjectionMatrix();
camera.position.z = 500;

// Add camera controls
const tbControls = new THREE.TrackballControls(camera, renderer.domElement);
tbControls.minDistance = 101;
tbControls.rotateSpeed = 5;
tbControls.zoomSpeed = 0.8;

// Kick-off renderer
(function animate() { // IIFE
    // Frame cycle
    tbControls.update();
    renderer.render(scene, camera);
    requestAnimationFrame(animate);
})();
```