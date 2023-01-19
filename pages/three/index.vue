<template>
  <div class="content">
    <canvas id="c"></canvas>
  </div>
</template>
<script>
export default {
  data() {
    return {};
  },
  mounted() {
    this.getThree();
  },
  methods: {
    getThree() {
      // 画一条线
      // const THREE = this.$three;

      // const renderer = new THREE.WebGLRenderer();
      // renderer.setSize(window.innerWidth, window.innerHeight);
      // document.body.appendChild(renderer.domElement);
      // const camera = new THREE.PerspectiveCamera(
      //   45,
      //   window.innerWidth / window.innerHeight,
      //   1,
      //   500
      // );
      // camera.position.set(0, 0, 100);
      // camera.lookAt(0, 0, 0);
      // const scene = new THREE.Scene();
      // // 接下来我们要做的是定义一种材料。对于线条，我们必须使用LineBasicMaterial或LineDashedMaterial。
      // //create a blue LineBasicMaterial
      // const material = new THREE.LineBasicMaterial({ color: 0x0000ff });
      // // 在材料之后，我们将需要一个带有一些顶点的几何体：
      // const points = [];
      // points.push(new THREE.Vector3(-10, 0, 0));
      // points.push(new THREE.Vector3(0, 10, 0));
      // points.push(new THREE.Vector3(10, 0, 0));
      // const geometry = new THREE.BufferGeometry().setFromPoints(points);
      // // 请注意，线是在每对连续的顶点之间绘制的，但不是在第一个和最后一个之间绘制的（该线未闭合。）
      // // 现在我们有了两条线和一种材料的点，我们可以将它们放在一起形成一条线。
      // const line = new THREE.Line(geometry, material);
      // // 剩下的就是将它添加到场景并调用render。
      // scene.add(line);
      // renderer.render(scene, camera);

      const THREE = this.$three;
      const canvas = document.querySelector("#c");
      const renderer = new THREE.WebGLRenderer({ canvas });
      const fov = 75;
      const aspect = 2; // 画布默认
      const near = 0.1;
      const far = 5;
      const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
      // 设置camera纵横比
      if (this.resizeRendererToDisplaySize(renderer)) {
        const canvas = renderer.domElement;
        camera.aspect = canvas.clientWidth / canvas.clientHeight;
        camera.updateProjectionMatrix();
      }
      camera.position.z = 4;
      // camera.position.x= 0
      // camera.position.y= 1
      const scene = new THREE.Scene();
      const boxWidth = 1;
      const boxHeight = 1;
      const boxDepth = 1;
      const geometry1 = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);
      const radius1 = 1; // ui: radius
      const segments = 24; // ui: segments
      const geometry2 = new THREE.CircleGeometry(radius1, segments);
      const radius2 = 1; // ui: radius
      const height = 2; // ui: height
      const radialSegments = 16; // ui: radialSegments
      const geometry3 = new THREE.ConeGeometry(radius2, height, radialSegments);
      // MeshBasicMaterial不受灯光影响。让我们将其更改MeshPhongMaterial为受灯光影响的 a。
      // const material = new THREE.MeshPhongMaterial({ color: 0x44aa88 });
      //然后我们创建一个Mesh. A Meshin three 表示三个事物的组合
      // A Geometry（物体的形状）
      // A Material（如何绘制对象，闪亮的还是平坦的，什么颜色，要应用什么纹理等。）
      // 场景中该对象相对于其父对象的位置、方向和比例。在下面的代码中，父级是场景。
      // const cube = new THREE.Mesh(geometry, material);
      const cubes = [
        this.makeInstance(geometry1, 0x44aa88, 0),
        this.makeInstance(geometry2, 0x8844aa, -2),
        this.makeInstance(geometry3, 0xaa8844, 2),
      ];

      // 然后我们可以通过调用渲染器的渲染函数并将场景和相机传递给它来渲染场​​景
      const color = 0xffffff;
      const intensity = 1;
      const light = new THREE.DirectionalLight(color, intensity);
      light.position.set(0, 0, 10);
      // 最后我们将该网格添加到场景中
      cubes.forEach((cube, ndx) => {
        scene.add(cube);
      });
      scene.add(light);
      console.log(scene, "scene");
      renderer.render(scene, camera);
      const render = (time) => {
        time *= 0.001; // 将时间转换为秒
        cubes.forEach((cube, ndx) => {
          const speed = 1 + ndx * 0.1;
          const rot = time * speed;
          cube.rotation.x = rot;
          cube.rotation.y = rot;
        });
        renderer.render(scene, camera);
        window.requestAnimationFrame(render);
      };
      window.requestAnimationFrame(render);
    },
    makeInstance(geometry, color, x) {
      const THREE = this.$three;
      const scene = new THREE.Scene();
      const material = new THREE.MeshPhongMaterial({ color });
      const cube = new THREE.Mesh(geometry, material);
      // scene场景
      // A Geometry（物体的形状）
      // A Material（如何绘制对象，闪亮的还是平坦的，什么颜色，要应用什么纹理等。）
      // cube通过geometry和material得到网格
      scene.add(cube);
      cube.position.x = x;
      return cube;
    },
    resizeRendererToDisplaySize(renderer) {
      //请注意，我们检查画布是否确实需要调整大小。调整画布大小是画布规范中一个有趣的部分，如果它已经是我们想要的大小，最好不要设置相同的大小。
      // 一旦我们知道是否需要调整大小，我们就会调用renderer.setSize并传入新的宽度和高度。最后通过很重要false。 render.setSize默认情况下设置画布的 CSS 大小但这样做不是我们想要的。我们希望浏览器继续按照它对所有其他元素的方式工作，即使用 CSS 来确定元素的显示大小。我们不希望三个元素使用的画布与其他元素不同。
      // 请注意，如果画布已调整大小，我们的函数将返回 true。我们可以用它来检查是否还有其他我们应该更新的东西。让我们修改渲染循环以使用新函数
      const canvas = renderer.domElement;
      const width = canvas.clientWidth;
      const height = canvas.clientHeight;
      const needResize = canvas.width !== width || canvas.height !== height;
      if (needResize) {
        renderer.setSize(width, height, false);
      }
      return needResize;
    },
  },
};
</script>
<style lang='scss' scoped>
.content {
  width: 100vw;
  height: 100vh;
  #c {
    width: 100%;
    height: calc(100% - 5px);
    display: block;
  }
}
</style>