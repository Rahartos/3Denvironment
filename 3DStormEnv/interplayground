const createScene = () => {
            var scene = new BABYLON.Scene(engine);
            var camera = new BABYLON.ArcRotateCamera("Camera", 0, 0, 10, new BABYLON.Vector3(0, 0, 0), scene);
            camera.setPosition(new BABYLON.Vector3(20, 200, 400));
            camera.attachControl(canvas, true);

            new BABYLON.AxesViewer(scene, 50);
            const localAxes = new BABYLON.AxesViewer(scene, 50);

            camera.upperBetaLimit = (Math.PI / 2) * 0.99;

            // Light
            var light = new BABYLON.PointLight("omni", new BABYLON.Vector3(50, 200, 0), scene);

            //Materials
            var groundMaterial = new BABYLON.StandardMaterial("ground", scene);
            groundMaterial.specularColor = BABYLON.Color3.Black();

            var redMat = new BABYLON.StandardMaterial("ground", scene);
            redMat.diffuseColor = new BABYLON.Color3(0.4, 0.4, 0.4);
            redMat.specularColor = new BABYLON.Color3(0.4, 0.4, 0.4);
            redMat.emissiveColor = BABYLON.Color3.Red();

            var blueMat = new BABYLON.StandardMaterial("ground", scene);
            blueMat.diffuseColor = new BABYLON.Color3(0.4, 0.4, 0.4);
            blueMat.specularColor = new BABYLON.Color3(0.4, 0.4, 0.4);
            blueMat.emissiveColor = BABYLON.Color3.Blue();

            /*************************************Meshes****************************************/
            // Ground
            var ground = BABYLON.MeshBuilder.CreateGround("ground", {width:1000, height:1000}, scene, false);
            ground.material = groundMaterial;

            // Meshes
            var redSphere = BABYLON.MeshBuilder.CreateSphere("red", {diameter:20}, scene);
            redSphere.material = redMat;
            redSphere.position.y = 10;
            redSphere.position.x -= 100;

            var blueBox = BABYLON.MeshBuilder.CreateBox("blue", {size:20}, scene);
            blueBox.material = blueMat;
            blueBox.position.x += 100;
            blueBox.position.y = 10;

            var startingPoint;
            var currentMesh;

        var getGroundPosition = function () {
            var pickinfo = scene.pick(scene.pointerX, scene.pointerY, function (mesh) { return mesh == ground; });
            if (pickinfo.hit) {
                return pickinfo.pickedPoint;
            }

            return null;
        }

        var pointerDown = function (mesh) {
                currentMesh = mesh;
                startingPoint = getGroundPosition();
                if (startingPoint) { // we need to disconnect camera from canvas
                    setTimeout(function () {
                        camera.detachControl(canvas);
                    }, 0);
                }
        }

        var pointerUp = function () {
            if (startingPoint) {
                camera.attachControl(canvas, true);
                startingPoint = null;
                return;
            }
        }

        var pointerMove = function () {
            if (!startingPoint) {
                return;
            }
            var current = getGroundPosition();
            if (!current) {
                return;
            }

            var diff = current.subtract(startingPoint);
            currentMesh.position.addInPlace(diff);

            startingPoint = current;

        }

    scene.onPointerObservable.add((pointerInfo) => {      		
        switch (pointerInfo.type) {
			case BABYLON.PointerEventTypes.POINTERDOWN:
				if(pointerInfo.pickInfo.hit && pointerInfo.pickInfo.pickedMesh != ground) {
                    pointerDown(pointerInfo.pickInfo.pickedMesh)
                }
				break;
			case BABYLON.PointerEventTypes.POINTERUP:
                    pointerUp();
				break;
			case BABYLON.PointerEventTypes.POINTERMOVE:          
                    pointerMove();
				break;
        }
    });

    BABYLON.SceneLoader.ImportMesh(
        "",
        "https://models.babylonjs.com/Channel9/",
        "n_10.stl",
        scene,
        function (meshes) {

        });

    // Move the light with the camera
    scene.registerBeforeRender(function () {
        light.position = camera.position;
    });

    return scene;
    }
    