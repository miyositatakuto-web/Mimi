<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>音羽ミミ MRテスト</title>
  <style>body { margin: 0; }</style>
  <!-- three.js(3D表示のライブラリ)をネットから読み込む設定 -->
  <script type="importmap">
  {
    "imports": {
      "three": "https://cdn.jsdelivr.net/npm/three@0.170.0/build/three.module.js",
      "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.170.0/examples/jsm/"
    }
  }
  </script>
</head>
<body>
<script type="module">
import * as THREE from 'three';
import { ARButton } from 'three/addons/webxr/ARButton.js';

// 3D空間(シーン)とカメラを用意
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(70, innerWidth / innerHeight, 0.01, 20);

// 描画の準備。alpha: true で背景を透明にし、現実の部屋が見えるようにする
const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
renderer.setPixelRatio(devicePixelRatio);
renderer.setSize(innerWidth, innerHeight);
renderer.xr.enabled = true; // VR/MRを有効にする
document.body.appendChild(renderer.domElement);

// 「START AR」ボタンを画面に出す
document.body.appendChild(ARButton.createButton(renderer));

// ライト(これがないと立方体が真っ暗になる)
scene.add(new THREE.HemisphereLight(0xffffff, 0x444444, 3));

// 立方体を作る(1辺20cm、ピンク色)
const cube = new THREE.Mesh(
  new THREE.BoxGeometry(0.2, 0.2, 0.2),
  new THREE.MeshStandardMaterial({ color: 0xff77aa })
);
// 位置:床から1.2mの高さ、開始位置の80cm前
cube.position.set(0, 1.2, -0.8);
scene.add(cube);

// 毎フレーム実行される処理:立方体を少しずつ回す
renderer.setAnimationLoop(() => {
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
});

// 画面サイズが変わったときの調整
addEventListener('resize', () => {
  camera.aspect = innerWidth / innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth, innerHeight);
});
</script>
</body>
</html>