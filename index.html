<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Mini Call of Duty - Browser Edition</title>
    <style>
        body { margin: 0; overflow: hidden; background: #000; font-family: Arial, sans-serif; }
        
        /* The Crosshair */
        #crosshair {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 20px;
            height: 20px;
            background: transparent;
            border: 2px solid rgba(0, 255, 0, 0.7);
            border-radius: 50%;
            transform: translate(-50%, -50%);
            pointer-events: none;
            z-index: 10;
        }
        #crosshair::after {
            content: '';
            position: absolute;
            top: 50%; left: 50%;
            width: 4px; height: 4px;
            background: red;
            transform: translate(-50%, -50%);
            border-radius: 50%;
        }

        /* UI Overlay */
        #ui {
            position: absolute;
            top: 20px;
            left: 20px;
            color: #0f0;
            font-size: 24px;
            font-weight: bold;
            z-index: 10;
            text-shadow: 1px 1px 2px black;
        }
        
        /* Instructions Overlay */
        #instructions {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            color: white;
            text-align: center;
            background: rgba(0,0,0,0.8);
            padding: 20px;
            border: 1px solid #444;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <div id="ui">SCORE: <span id="score">0</span> | HEALTH: <span id="health">100</span></div>
    <div id="crosshair"></div>
    
    <div id="instructions">
        <h1>MINI CALL OF DUTY</h1>
        <p>Click to Start</p>
        <p>WASD to Move | Mouse to Look | Click to Shoot</p>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <script>
        // --- GAME VARIABLES ---
        let camera, scene, renderer;
        let bullets = [];
        let enemies = [];
        let score = 0;
        let playerHealth = 100;
        let lastTime = performance.now();
        let moveForward = false, moveBackward = false, moveLeft = false, moveRight = false;
        let velocity = new THREE.Vector3();
        let direction = new THREE.Vector3();
        let gun;
        let isGameActive = false;

        // Settings
        const MOVEMENT_SPEED = 150.0;
        const ENEMY_SPEED = 20.0;
        const BULLET_SPEED = 400.0;
        
        const uiScore = document.getElementById('score');
        const uiHealth = document.getElementById('health');
        const instructions = document.getElementById('instructions');

        init();
        animate();

        function init() {
            // 1. Setup Scene
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x111111);
            scene.fog = new THREE.Fog(0x111111, 0, 700);

            // 2. Setup Camera
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 1, 1000);
            camera.position.y = 10; // Player height

            // 3. Setup Lights
            const light = new THREE.HemisphereLight(0xeeeeee, 0x777777);
            light.position.set(0.5, 1, 0.75);
            scene.add(light);

            // 4. Create the Map (Floor)
            const floorGeometry = new THREE.PlaneGeometry(2000, 2000, 100, 100);
            floorGeometry.rotateX(-Math.PI / 2);
            const floorMaterial = new THREE.MeshBasicMaterial({ color: 0x222222, wireframe: true });
            const floor = new THREE.Mesh(floorGeometry, floorMaterial);
            scene.add(floor);

            // 5. Create the Weapon (Simple Box)
            const gunGeometry = new THREE.BoxGeometry(0.5, 0.5, 3);
            const gunMaterial = new THREE.MeshStandardMaterial({ color: 0x333333 });
            gun = new THREE.Mesh(gunGeometry, gunMaterial);
            gun.position.set(2, -1.5, -3); // Offset gun to bottom right
            camera.add(gun); // Attach gun to camera
            scene.add(camera);

            // 6. Renderer
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setPixelRatio(window.devicePixelRatio);
            renderer.setSize(window.innerWidth, window.innerHeight);
            document.body.appendChild(renderer.domElement);

            // 7. Event Listeners
            document.addEventListener('keydown', onKeyDown);
            document.addEventListener('keyup', onKeyUp);
            document.addEventListener('mousedown', onMouseDown);
            window.addEventListener('resize', onWindowResize);

            // Pointer Lock (Mouse Capture)
            instructions.addEventListener('click', function () {
                document.body.requestPointerLock();
            });

            document.addEventListener('pointerlockchange', function() {
                if (document.pointerLockElement === document.body) {
                    isGameActive = true;
                    instructions.style.display = 'none';
                } else {
                    isGameActive = false;
                    instructions.style.display = 'block';
                }
            });

            // Mouse Movement logic
            document.body.addEventListener('mousemove', (event) => {
                if (!isGameActive) return;
                camera.rotation.y -= event.movementX * 0.002;
                camera.rotation.x -= event.movementY * 0.002;
                // Clamp looking up/down
                camera.rotation.x = Math.max(-Math.PI / 2, Math.min(Math.PI / 2, camera.rotation.x));
            });
        }

        function onMouseDown() {
            if (!isGameActive) return;
            shoot();
        }

        function shoot() {
            // Create Bullet
            const geometry = new THREE.SphereGeometry(0.2, 8, 8);
            const material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
            const bullet = new THREE.Mesh(geometry, material);

            // Set bullet position to gun muzzle
            const vector = new THREE.Vector3();
            gun.getWorldPosition(vector);
            bullet.position.copy(vector);

            // Set bullet direction based on where camera is looking
            const dir = new THREE.Vector3();
            camera.getWorldDirection(dir);
            bullet.velocity = dir.multiplyScalar(BULLET_SPEED);

            bullets.push(bullet);
            scene.add(bullet);

            // Recoil Animation
            gun.position.z += 0.5;
            setTimeout(() => { gun.position.z -= 0.5; }, 50);
        }

        function spawnEnemy() {
            if (enemies.length > 10) return; // Limit enemies

            const geometry = new THREE.BoxGeometry(4, 10, 4);
            const material = new THREE.MeshStandardMaterial({ color: 0xff0000 });
            const enemy = new THREE.Mesh(geometry, material);

            // Random spawn position
            const angle = Math.random() * Math.PI * 2;
            const radius = 50 + Math.random() * 100;
            enemy.position.x = camera.position.x + Math.cos(angle) * radius;
            enemy.position.z = camera.position.z + Math.sin(angle) * radius;
            enemy.position.y = 5;

            enemies.push(enemy);
            scene.add(enemy);
        }

        function onKeyDown(event) {
            switch (event.code) {
                case 'ArrowUp': case 'KeyW': moveForward = true; break;
                case 'ArrowLeft': case 'KeyA': moveLeft = true; break;
                case 'ArrowDown': case 'KeyS': moveBackward = true; break;
                case 'ArrowRight': case 'KeyD': moveRight = true; break;
            }
        }

        function onKeyUp(event) {
            switch (event.code) {
                case 'ArrowUp': case 'KeyW': moveForward = false; break;
                case 'ArrowLeft': case 'KeyA': moveLeft = false; break;
                case 'ArrowDown': case 'KeyS': moveBackward = false; break;
                case 'ArrowRight': case 'KeyD': moveRight = false; break;
            }
        }

        function onWindowResize() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        }

        function animate() {
            requestAnimationFrame(animate);

            if (!isGameActive) return;

            const time = performance.now();
            const delta = (time - lastTime) / 1000;
            lastTime = time;

            // --- Player Movement ---
            velocity.x -= velocity.x * 10.0 * delta;
            velocity.z -= velocity.z * 10.0 * delta;

            direction.z = Number(moveForward) - Number(moveBackward);
            direction.x = Number(moveRight) - Number(moveLeft);
            direction.normalize(); // Ensure consistent speed in all directions

            if (moveForward || moveBackward) velocity.z -= direction.z * MOVEMENT_SPEED * delta;
            if (moveLeft || moveRight) velocity.x -= direction.x * MOVEMENT_SPEED * delta;

            // Apply movement relative to camera direction
            camera.translateX(-velocity.x * delta);
            camera.translateZ(-velocity.z * delta);
            camera.position.y = 10; // Keep on floor

            // --- Bullet Physics ---
            for (let i = bullets.length - 1; i >= 0; i--) {
                let b = bullets[i];
                b.position.addScaledVector(b.velocity, delta);

                // Remove bullet if too far
                if (b.position.distanceTo(camera.position) > 500) {
                    scene.remove(b);
                    bullets.splice(i, 1);
                }
            }

            // --- Enemy Logic (AI & Collision) ---
            if (Math.random() < 0.02) spawnEnemy(); // 2% chance per frame to spawn

            for (let i = enemies.length - 1; i >= 0; i--) {
                let e = enemies[i];
                
                // Move towards player
                let directionToPlayer = new THREE.Vector3().subVectors(camera.position, e.position).normalize();
                e.position.addScaledVector(directionToPlayer, ENEMY_SPEED * delta);
                e.lookAt(camera.position);

                // Collision: Enemy hits Player
                if (e.position.distanceTo(camera.position) < 5) {
                    playerHealth -= 1;
                    uiHealth.innerText = Math.floor(playerHealth);
                    // Flash red
                    document.body.style.boxShadow = "inset 0 0 50px red";
                    setTimeout(() => document.body.style.boxShadow = "none", 100);
                    
                    if(playerHealth <= 0) {
                        alert("GAME OVER! Score: " + score);
                        location.reload();
                    }
                }

                // Collision: Bullet hits Enemy
                for (let j = bullets.length - 1; j >= 0; j--) {
                    if (bullets[j].position.distanceTo(e.position) < 3) { // Hitbox size
                        // Kill enemy
                        scene.remove(e);
                        enemies.splice(i, 1);
                        
                        // Remove bullet
                        scene.remove(bullets[j]);
                        bullets.splice(j, 1);
                        
                        // Update Score
                        score += 100;
                        uiScore.innerText = score;
                        break; 
                    }
                }
            }

            renderer.render(scene, camera);
        }
    </script>
</body>
</html>
