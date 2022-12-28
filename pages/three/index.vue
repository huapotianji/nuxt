<template>
  <div>
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
      camera.position.z = 2;
      const scene = new THREE.Scene();
      const boxWidth = 1;
      const boxHeight = 1;
      const boxDepth = 1;
      const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth);
      // MeshBasicMaterial不受灯光影响。让我们将其更改MeshPhongMaterial为受灯光影响的 a。
      const material = new THREE.MeshPhongMaterial({ color: 0x44aa88 });
      //然后我们创建一个Mesh. A Meshin three 表示三个事物的组合
      // A Geometry（物体的形状）
      // A Material（如何绘制对象，闪亮的还是平坦的，什么颜色，要应用什么纹理等。）
      // 场景中该对象相对于其父对象的位置、方向和比例。在下面的代码中，父级是场景。
      const cube = new THREE.Mesh(geometry, material);
      
      // 然后我们可以通过调用渲染器的渲染函数并将场景和相机传递给它来渲染场​​景
      const color = 0xffffff;
      const intensity = 1;
      const light = new THREE.DirectionalLight(color, intensity);
      light.position.set(-1, 2, 4);
      // 最后我们将该网格添加到场景中
      scene.add(cube,light)
      console.log(scene,'scene')
      renderer.render(scene, camera);
      const render = (time) => {
        time *= 0.001; // 将时间转换为秒
        cube.rotation.x = time;
        cube.rotation.y = time;
        renderer.render(scene, camera);
        window.requestAnimationFrame(render);
      };
      window.requestAnimationFrame(render);
    },
  },
};
</script>
<style lang='scss' scoped>
</style>