<template>
  <div>
    <canvas ref="game" id="canvas" width="400" height="400"></canvas>
  </div>
</template>

<script>
import { onMounted, ref } from "vue";
export default {
  setup() {
    const game = ref(null);

    onMounted(() => {
      _main();
    });

    const Paddle = () => {
      const obj = {
        x: 150,
        y: 320,
      };

      obj.toLeft = () => {
        obj.x -= 5;
      };
      obj.toRight = () => {
        obj.x += 5;
      };
      return obj;
    };

    const GameCanvas = () => {
      const canvas = game.value;
      const ctx = canvas.getContext("2d");
      const g = {
        canvas,
        ctx,
      };

      setInterval(() => {
        // 更新
        g.update();

        //渲染
        g.draw();
      }, 1000 / 30);

      return g;
    };

    const _main = () => {
      const paddle = Paddle();

      const game = GameCanvas();

      game.ctx.fillStyle = "orange";
      game.ctx.fillRect(paddle.x, 320, 100, 15);

      let toLeft = false;
      let toRight = false;

      //控制挡板左右
      document.addEventListener("keydown", (e) => {
        const key = e.key;
        if (key === "s") {
          toLeft = true;
        } else if (key === "d") {
          toRight = true;
        }
      });

      document.addEventListener("keyup", (e) => {
        const key = e.key;
        if (key === "s") {
          toLeft = false;
        } else if (key === "d") {
          toRight = false;
        }
      });

      game.update = () => {
        if (toLeft) {
          paddle.toLeft();
        } else if (toRight) {
          paddle.toRight();
        }
      };

      game.draw = () => {
        game.ctx.clearRect(0, 0, game.canvas.width, game.canvas.height); // 清除整个画布
        game.ctx.fillRect(paddle.x, 320, 100, 15);
      };
    };

    return { game };
  },
};
</script>

<style lang="scss" scoped>
canvas {
  border: 1px solid #ccc;
}
</style>