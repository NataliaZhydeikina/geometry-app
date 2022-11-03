<template>
  <div>
    <div id="app">
      <div
        id="container"
        class="absolute text-white text-center max-w-2xl px-6"
      >
        <h1
          id="name"
          class="font-space-mono text-sm tracking-wide opacity-0"
          style="transform: translateY(30px)"
        >
          Nataliia
        </h1>
        <p
          id="paragraph"
          class="font-exo text-2xl font-bold opacity-0"
          style="transform: translateY(30px)"
        >
          One with everlasting desire for the unknown & untold
        </p>
        <a
          href="https://codepen.io/nataliazhydeikina/full/BaVybYE"
          id="btn"
          class="border px-4 py-2 rounded-lg text-sm font-space-mono mt-8 uppercase hover:bg-white hover:text-gray-800 inline-block opacity-0"
          >View Work</a
        >
      </div>
    </div>
    <canvas id="canvas" ref="canvas"></canvas>
  </div>
</template>

<style>
html,
body {
  margin: 0;
  height: 100%;
  overflow: hidden;
  --moz-osx-font-smoothing: grayscale;
  --webkit-font-smoothing: antialiased;
}
#canvas {
  display: block;
  width: 100%;
  height: 100%;
}
#app > div {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.font-exo {
  font-family: "Exo 2", sans-serif;
  font-style: italic;
  text-transform: uppercase;
}
.font-space-mono {
  font-family: "Space Mono", monospace;
}
</style>

<script>
import gsap from "gsap";
import {
  WebGLRenderer,
  Scene,
  Clock,
  PlaneBufferGeometry,
  BufferAttribute,
  MeshPhongMaterial,
  DoubleSide,
  Mesh,
  PerspectiveCamera,
  Raycaster,
  DirectionalLight,
  BufferGeometry,
  PointsMaterial,
  Points,
  Float32BufferAttribute,
} from "three";

export default {
  name: "IndexPage",
  mounted() {
    const dat = require("dat.gui");
    const renderer = new WebGLRenderer({ canvas: this.$refs.canvas });
    // const gui = new dat.GUI();
    const scene = new Scene();
    const clock = new Clock();
    const world = {
      plane: {
        width: 400,
        height: 400,
        widthSegment: 50,
        heightSegment: 50,
      },
    };
    const geometry = new PlaneBufferGeometry(
      world.plane.width,
      world.plane.height,
      world.plane.widthSegment,
      world.plane.heightSegment
    );
    const material = new MeshPhongMaterial({
      side: DoubleSide,
      flatShading: true,
      vertexColors: true,
    });
    const plane = new Mesh(geometry, material);
    scene.add(plane);
    function generatePlane() {
      plane.geometry.dispose();
      const aspectRation = innerWidth / innerHeight;
      world.plane.width = Math.max(
        world.plane.width,
        world.plane.width * aspectRation * 0.25
      );
      world.plane.widthSegment = Math.max(
        world.plane.widthSegment,
        world.plane.widthSegment * aspectRation * 0.25
      );
      world.plane.width;
      plane.geometry = new PlaneBufferGeometry(
        world.plane.width,
        world.plane.height,
        world.plane.widthSegment,
        world.plane.heightSegment
      );
      console.log(plane.geometry);
      const { array } = plane.geometry.attributes.position;
      const colors = [];
      const randomValues = [];
      for (let i = 0; i < array.length; i++) {
        if (i % 3 === 0) {
          array[i] += (Math.random() - 0.5) * 3;
          array[i + 1] += (Math.random() - 0.5) * 3;
          array[i + 2] += (Math.random() - 0.5) * 3;
          colors.push(0, 0.19, 0.4);
        }
        randomValues.push(Math.random() * Math.PI * 2);
      }
      plane.geometry.attributes.color = new BufferAttribute(
        new Float32Array(colors),
        3
      );
      // plane.geometry.setAttribute(
      //   "color",
      //   new BufferAttribute(new Float32Array(colors), 3)
      // );
      plane.geometry.attributes.position.originalPosition =
        plane.geometry.attributes.position.array;
      plane.geometry.attributes.position.randomValues = randomValues;
    }
    generatePlane();
    // gui.add(world.plane, "width", 0, 500).onChange(generatePlane);
    // gui.add(world.plane, "height", 0, 500).onChange(generatePlane);
    // gui.add(world.plane, "widthSegment", 0, 100).onChange(generatePlane);
    // gui.add(world.plane, "heightSegment", 0, 100).onChange(generatePlane);
    const camera = new PerspectiveCamera(75, innerWidth / innerHeight, 1, 1000);
    camera.position.set(0, 0, 50);
    const raycaster = new Raycaster();
    const directionalLight = new DirectionalLight(0xffffff, 1);
    directionalLight.position.set(0, -1, 1);
    scene.add(directionalLight);
    const backDirectionalLight = new DirectionalLight(0xffffff, 1);
    backDirectionalLight.position.set(0, -1, -1);
    scene.add(backDirectionalLight);
    renderer.setSize(innerWidth, innerHeight);
    renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
    renderer.render(scene, camera);
    addEventListener("resize", () => {
      camera.aspect = innerWidth / innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(innerWidth, innerHeight);
      renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
    });
    const starGeometry = new BufferGeometry();
    const starMaterial = new PointsMaterial({ color: 0xffffff });
    const starVertices = [];
    for (let i = 0; i < 1000; i++) {
      const x = (Math.random() - 0.5) * 2000;
      const y = (Math.random() - 0.5) * 2000;
      const z = (Math.random() - 0.5) * 2000;
      starVertices.push(x, y, z);
    }
    starGeometry.attributes.position = new Float32BufferAttribute(
      starVertices,
      3
    );
    // starGeometry.setAttribute(
    //   "position",
    //   new Float32BufferAttribute(starVertices, 3)
    // );
    const stars = new Points(starGeometry, starMaterial);
    scene.add(stars);
    gsap.to("#name", {
      opacity: 1,
      duration: 1.5,
      y: 0,
      ease: "expo",
    });
    gsap.to("#paragraph", {
      opacity: 1,
      duration: 1.5,
      delay: 0.3,
      y: 0,
      ease: "expo",
    });
    gsap.to("#btn", {
      opacity: 1,
      duration: 1.5,
      delay: 0.6,
      y: 0,
      ease: "expo",
    });
    document.querySelector("#btn").addEventListener("click", (e) => {
      e.preventDefault();
      gsap.to("#container", {
        opacity: 0,
      });
      let tl = gsap.timeline();
      tl.to(camera.position, {
        z: 25,
        ease: "power2.inOut",
        duration: 2,
      }).to(camera.position, {
        y: 1000,
        ease: "power3.in",
        duration: 2,
        onComplete: () => {
          this.$router.push("/work");
          //window.location = "https://codepen.io/nataliazhydeikina/full/BaVybYE";
        },
      });
      gsap.to(camera.rotation, {
        x: 1.57,
        ease: "power2.inOut",
        duration: 2,
      });
    });
    const mouse = {
      x: undefined,
      y: undefined,
    };
    addEventListener("mousemove", (event) => {
      mouse.x = (event.clientX / innerWidth) * 2 - 1;
      mouse.y = -(event.clientY / innerHeight) * 2 + 1;
    });
    function tick() {
      renderer.render(scene, camera);
      raycaster.setFromCamera(mouse, camera);
      const { array, originalPosition, randomValues } =
        plane.geometry.attributes.position;
      for (let i = 0; i < array.length; i += 3) {
        //x
        array[i] =
          originalPosition[i] +
          Math.cos(clock.getElapsedTime() + randomValues[i]) * 0.01;
        //y
        array[i + 1] =
          originalPosition[i + 1] +
          Math.sin(clock.getElapsedTime() + randomValues[i + 1]) * 0.01;
      }
      plane.geometry.attributes.position.needsUpdate = true;
      const intersects = raycaster.intersectObject(plane);
      if (intersects.length > 0) {
        let { color } = intersects[0].object.geometry.attributes;
        color.setX(intersects[0].face.a, 0.1);
        color.setY(intersects[0].face.a, 0.5);
        color.setZ(intersects[0].face.a, 1);
        color.setX(intersects[0].face.b, 0.1);
        color.setY(intersects[0].face.b, 0.5);
        color.setZ(intersects[0].face.b, 1);
        color.setX(intersects[0].face.c, 0.1);
        color.setY(intersects[0].face.c, 0.5);
        color.setZ(intersects[0].face.c, 1);
        color.needsUpdate = true;
        const initialColor = {
          r: 0,
          g: 0.19,
          b: 0.4,
        };
        const hoverColor = {
          r: 0.1,
          g: 0.5,
          b: 1,
        };
        gsap.to(hoverColor, {
          r: initialColor.r,
          g: initialColor.g,
          b: initialColor.b,
          onUpdate: () => {
            color.setX(intersects[0].face.a, hoverColor.r);
            color.setY(intersects[0].face.a, hoverColor.g);
            color.setZ(intersects[0].face.a, hoverColor.b);
            color.setX(intersects[0].face.b, hoverColor.r);
            color.setY(intersects[0].face.b, hoverColor.g);
            color.setZ(intersects[0].face.b, hoverColor.b);
            color.setX(intersects[0].face.c, hoverColor.r);
            color.setY(intersects[0].face.c, hoverColor.g);
            color.setZ(intersects[0].face.c, hoverColor.b);
            color.needsUpdate = true;
          },
        });
      }
      stars.rotation.x += 0.005;
      // controls.update();
      requestAnimationFrame(tick);
    }
    tick();
  },
};
</script>
