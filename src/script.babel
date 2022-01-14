import { OrbitControls } from "https://threejsfundamentals.org/threejs/resources/threejs/r110/examples/jsm/controls/OrbitControls.js";

class THREEScene {
  constructor(container = document.body) {
    this.container = container;

    this.setup();
    this.camera();
    this.addObjects();
    this.eventListeners();
    this.settings();
    this.render();
    this.animate();
  }

  settings() {
    this.settings = {
      blur: 0,
      speed: 0.5,
      noiseFreq: 1.0
    };
  }

  setup() {
    this.clock = new THREE.Clock();
    this.mouse = new THREE.Vector2();
    this.scene = new THREE.Scene();
    this.renderer = new THREE.WebGLRenderer({
      antialias: true
    });
    this.renderer.setSize(this.viewport.width, this.viewport.height);
    this.renderer.setPixelRatio = window.devicePixelRatio;
    this.renderer.setClearColor(0xeadcc8, 1);
    this.container.appendChild(this.renderer.domElement);
  }

  camera() {
    const FOV = 50;
    const NEAR = 0.001;
    const FAR = 100;
    const ASPECT_RATIO = this.viewport.aspectRatio;

    this.camera = new THREE.PerspectiveCamera(FOV, ASPECT_RATIO, NEAR, FAR);
    this.camera.position.set(0, 0, 20);
    this.camera.aspect = this.viewport.width / this.viewport.height;
    this.camera.updateProjectionMatrix();
  }

  lights() {
    //const ambientLight = new THREE.AmbientLight(0x404040);
  }

  addObjects() {
    this.geometry = new THREE.IcosahedronGeometry(4, 32);
    this.material = new THREE.ShaderMaterial({
      uniforms: {
        time: { value: 0 },
        u_factor: { value: 0.5 },
        u_opacity: { value: 0 }
      },
      vertexShader: document.getElementById("vertex").textContent,
      fragmentShader: document.getElementById("fragment").textContent
    });

    this.cube = new THREE.Points(this.geometry, this.material);
    this.cube.scale.set(0.5, 0.5, 0.5);
    this.scene.add(this.cube);
  }

  render() {
    this.camera.lookAt(this.scene.position);
    this.renderer.render(this.scene, this.camera);

    this.material.uniforms.time.value = this.clock.getElapsedTime();
    this.cube.rotation.y += 0.005;
    this.cube.rotation.x += 0.003;

    requestAnimationFrame(() => {
      this.render();
    });
  }

  animate() {
    gsap.set(".overlay", { scaleX: 0, transformOrigin: "center right" });
    gsap.set("h1", { x: -24 });

    const tl = gsap.timeline({
      delay: 1
    });

    tl.to("h1", {
      x: 0,
      opacity: 1
    })
      .fromTo(
        "li",
        {
          opacity: 0,
          y: 32
        },
        {
          opacity: 1,
          y: 0,
          stagger: 0.15,
          ease: "power2.out"
        },
        0
      )
      .to(
        ".overlay",
        {
          scaleX: 1,
          duration: 2.5,
          ease: "expo.inOut"
        },
        0
      )
      .set("canvas", {
        opacity: 1
      })
      .to(
        this.camera.position,
        {
          duration: 4,
          ease: "power3.inOut",
          z: 7,
          y: 0,
          x: 0
        },
        1
      )
      .to(
        ".overlay",
        {
          opacity: 0,
          duration: 4
        },
        "-=2.5"
      );

    const nav = document.querySelector("ul");

    nav.addEventListener("mouseenter", () => {
      gsap.to(this.material.uniforms.u_factor, {
        value: 1.0
      });
    });

    nav.addEventListener("mouseleave", () => {
      gsap.to(this.material.uniforms.u_factor, {
        value: 0.5
      });
    });
  }

  eventListeners() {
    window.addEventListener("resize", this.onWindowResize.bind(this));
  }

  onWindowResize() {
    this.camera.aspect = this.viewport.aspectRatio;
    this.camera.updateProjectionMatrix();
    this.renderer.setSize(this.viewport.width, this.viewport.height);
  }

  get viewport() {
    const width = this.container.clientWidth / 2;
    const height = this.container.clientHeight;
    const aspectRatio = width / height;

    this.aspectRatio = aspectRatio;

    return {
      width,
      height,
      aspectRatio
    };
  }
}

const scene = new THREEScene();
