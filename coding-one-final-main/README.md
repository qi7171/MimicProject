# coding-one-final
## Project description
### Butterfly Wind Chime
I created a 3d butterfly wind chime scene using three.js. 

Video link: https://www.youtube.com/watch?v=94eSoytSdIE
<img width="1203" alt="截屏2022-12-05 02 09 56" src="https://git.arts.ac.uk/storage/user/502/files/936afdbc-f733-49ea-b808-635477ea3765">

1. Animation: 1) the geometries animate in different rotation 2) particles rotate randomely in the screen

2. Mouse interaction: the wind chime circle will sound when hover the mouse on 
the random colored circle particles, and the color of hovered particle will change to red.

3. Sound: combined four sounds to create wind chime rhythmic.

4. different material to variables: shader, image, random color

5. ambient light and directional light




#### style.css:
```
  background-image: url('butterfly5.png');
    background: -moz-linear-gradient(top, #7a04eb, #120458); 
    background: -webkit-linear-gradient(top, #fe75fe, #fe75fe);
    background: linear-gradient(to top, #7a04eb, #fe75fe);
```
Description: adding an image to the background and set the background colour as gradient.


#### script.js:
```
var uniforms1 = {
      "time": { value: 1.0 }};        
    const params = [[ 'fragment_shader1', uniforms1 ], ];
```
Description: When declaring a uniform of a ShaderMaterial, it is declared by value or by object.


```
    const fov = 80;
    const aspect = window.innerWidth / window.innerHeight;
    const near = 0.1;
    const far = 1000;
    camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
    camera.position.z = 400;
    scene.add(camera);
```
Description: Set up the camera, we can edit the field of view of the camera.

```
 const audioListener = new THREE.AudioListener();
    audio = new THREE.Audio(audioListener);

    const audioLoader = new THREE.AudioLoader();
    audioLoader.load('wind.wav', (buffer) => {
      audio.setBuffer(buffer);
      audio.setLoop(true);
      
    });
```
Description: load the audio and play

```
 window.addEventListener("click", function() {
      if (!audio.isPlaying) 
        audio.play();
    })
```
Description: play audio by mouse click

```
 const fftSize = 32;
    analyser = new THREE.AudioAnalyser(audio, fftSize);
```
Description: audio analyser: get the average frequency of the sound


```
 circle = new THREE.Object3D();
 circle2 = new THREE.Object3D();
 circle3 = new THREE.Object3D();
 circle4 = new THREE.Object3D();
 skelet = new THREE.Object3D();
 particle = new THREE.Object3D();
 scene.add(circle);
 scene.add(circle2);
 scene.add(circle3);
 scene.add(circle4);
 scene.add(skelet);
 scene.add(particle);
```
Description: Create 3D object, and add on the scene

```

  var geometry = new THREE.TetrahedronGeometry(2, 1);
  var geometry2 = new THREE.IcosahedronGeometry(7, 0);
  var geometry4 = new THREE.OctahedronGeometry(16, 1);
  var geometry5 = new THREE.CircleGeometry(5,32);
  var geometry6 = new THREE.OctahedronGeometry(1.5,0);
  var geometry7 = new THREE.OctahedronGeometry(1,0);
  var geometry8 = new THREE.ConeGeometry( 3,9,6 );
  var geometry9 = new THREE.TetrahedronGeometry( 5,0);
```
 Description:  create some built in geometry. 
 it is a geometry (radius , detail)


```
 var myTextureLoader = new THREE.TextureLoader();
```
 Description:to texture the geometry, add a texture loader object to load the texture 
 
 
```
var texture = new THREE.MeshPhongMaterial({
       color: Math.random() * 0xff124f,
        shading: THREE.DobuleSide,
        wireframe: true
    });
```
Description: 
create a material: This defines how the surface of the object reflects light.
```THREE.MeshPhongMaterial```  it is using Phong here. 
```Math.random() * 0xff124f```is random color.
 ```shading: THREE.DobuleSide ``` the shading material is double side.
``` wireframe: true``` show the wireframe.

```
 var material = new THREE.ShaderMaterial( {
      uniforms: params[ 0 ][ 1 ],
      vertexShader: document.getElementById( 'vertexShader' ).textContent,
      fragmentShader: document.getElementById( params[ 0 ][ 0 ] ).textContent
	} );
```
Description: adding shader material 

```
var myTexture = myTextureLoader.load('butterfly9.png');
```
Description: load image as texture


```
    circle.rotation.x += 0.03;
    circle.rotation.y += 0.02; 
 ```
 Description: x and y rotation of circle


 ```
    var innerShape = new THREE.Mesh(geometry2, material);
    innerShape.scale.x = innerShape.scale.y = innerShape.scale.z = 14;
    circle.add(innerShape);
 ```
 Description: the innerShape call the shape- ```geometry2``` and material- ```material``` in scale 14. ```circle.add(innerShape)```  circle animation
 
 
  ```
    for (var i = 0; i < 800; i++) {
        var mesh = new THREE.Mesh(geometry, material2);
        mesh.position.set(Math.random() - 0.5, Math.random() - 0.5, Math.random() -  0.5).normalize();
        mesh.position.multiplyScalar(90 + (Math.random() * 700));
        mesh.rotation.set(Math.random() * 2, Math.random() * 2 , Math.random() * 2);
        particle.add(mesh);
    }
 ```
 Description: using a for loop to create 800 3D particles in random position and rotation.  
 ```var mesh = new THREE.Mesh(geometry, material2) ```  Create mesh from the shape and the material
 
 
  ```
  group = new THREE.Group();
  scene.add( group );
  for (var r = 0; r < 400; r++) {
    var particle2 = new THREE.Mesh( geometry5, new THREE.MeshPhongMaterial( { color: Math.random() * 0xff124f} ) );
	particle2.position.x = Math.random() *200-100;
	particle2.position.y = Math.random() *200-100;
	particle2.position.z = Math.random() *200-100;

	particle2.scale.x = Math.random() * 2;
	particle2.scale.y = Math.random() * 2;
	particle2.scale.z = Math.random() * 2;

    scene.add(particle2);
    group.add(particle2);
  } 
  ```
 Description: using for loop to create 400 particles in random position. ```group = new THREE.Group();``` group the particles. 
 ```particle2.position.x``` set the x position, ```particle2.scale.x = Math.random() * 2``` the scale of x position
 
 

```
 194-207:
    var ambientLight = new THREE.AmbientLight(0x7a04eb);
    scene.add(ambientLight);
    var dLight = [];
    dLight[0] = new THREE.DirectionalLight(0x7a04eb, 1);
    dLight[0].position.set(1, 0, 0);
```
 Description: Using ambient light and directional light.  
 ```var ambientLight = new THREE.AmbientLight(0x7a04eb);``` This purple light globally illuminates all objects in the scene equally.
 ```dLight[0] = new THREE.DirectionalLight(0x7a04eb, 1);``` DirectionalLight( color : purple, intensity : numeric value of the light's strength/intensity)
 ``` dLight[0].position.set(1, 0, 0) ``` directional light position

```function onPointerMove( event ){
if ( selectedObject ) {
	  selectedObject.material.color.set( '#ff124f' );
	  selectedObject = null;
      interactiveAudio.play();
	}}
```
Description: when the mouse hover on the selected object, it will change to red color, and will play sound. 

```
    particle.rotation.x += 0;
    particle.rotation.y -= 0.004;
    particle.rotation.z -= 0.002;
```
 Description: the x, y, z rotation of particles.
 
#### orbitControls.js:
This set of controls performs orbiting, dollying (zooming), and panning. It maintains
the "up" direction as +Y, unlike the TrackballControls. Touch on tablet and phones is supported.
Orbit - left mouse / touch: one finger move
Zoom - middle mouse, or mousewheel / touch: two finger spread or squish
Pan - right mouse, or arrow keys / touch: three finter swipe

This is a drop-in replacement for (most) TrackballControls used in examples.
That is, include this js file and wherever you see:
controls = new THREE.TrackballControls( camera );
controls.target.z = 150;
Simple substitute "OrbitControls" and the control should work as-is.

#### Sound.js:
Combining four sounds to create wind chime rhythmic. 

```
if( myClock.tick && myClock.playHead % windCount===4){
           chime.trigger();
	  windGain = 1; 
}
```
Description: The .tick method is a boolean belonging to the maxiClock object is either true or false.
the .playHead method keeps a running count of the number of beats
We're using modulo (%) to turn the playHead count in to a beat division
e.g. playHead%4 counts 0,1,2,3,0,1,2,3.
The if statement checks to see if there's a tick. If there is one, and the playHead%4 is equal to 0, we trigger a piano drum.   


    
    
    
    






















