# Satellites demo

**Install scripts**
- three - `https://unpkg.com/three@0.145.0/build/three.js`
- threeControls - `https://unpkg.com/three/examples/js/controls/TrackballControls.js`
- threeGlobe - `https://unpkg.com/three-globe@2.24.8/dist/three-globe.min.js`
- satellite - `https://unpkg.com/satellite.js/dist/satellite.min.js`

```js scripts=three,threeControls,threeGlobe,satellite
//hide
const EARTH_RADIUS_KM = 6371; // km
const SAT_SIZE = 80; // km
const TIME_STEP = 3 * 1000; // per frame

const Globe = new ThreeGlobe()
    .globeImageUrl('https://unpkg.com/three-globe/example/img/earth-blue-marble.jpg')
    .objectLat('lat')
    .objectLng('lng')
    .objectAltitude('alt');

const satGeometry = new THREE.OctahedronGeometry(SAT_SIZE * Globe.getGlobeRadius() / EARTH_RADIUS_KM / 2, 0);
const satMaterial = new THREE.MeshLambertMaterial({ color: 'palegreen', transparent: true, opacity: 0.7 });
Globe.objectThreeObject(() => new THREE.Mesh(satGeometry, satMaterial));

const result = await fetch('https://raw.githubusercontent.com/vasturiano/three-globe/master/example/satellites/space-track-leo.txt');
const rawData = await result.text();

const tleData = rawData.replace(/\r/g, '').split(/\n(?=[^12])/).map(tle => tle.split('\n'));
const satData = tleData.map(([name, ...tle]) => ({
satrec: satellite.twoline2satrec(...tle),
name: name.trim().replace(/^0 /, '')
}));

// time ticker
let time = new Date();
(function frameTicker() {
requestAnimationFrame(frameTicker);

time = new Date(+time + TIME_STEP);

// Update satellite positions
const gmst = satellite.gstime(time);
satData.forEach(d => {
    const eci = satellite.propagate(d.satrec, time);
    if (eci.position) {
    const gdPos = satellite.eciToGeodetic(eci.position, gmst);
    d.lat = satellite.radiansToDegrees(gdPos.latitude);
    d.lng = satellite.radiansToDegrees(gdPos.longitude);
    d.alt = gdPos.height / EARTH_RADIUS_KM
    }
});

Globe.objectsData(satData);
})();


//

// Setup renderer
const renderer = new THREE.WebGLRenderer();
renderer.setSize(500, 500);
htmlEl.innerHTML = '' // reset
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
camera.position.z = 400;

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