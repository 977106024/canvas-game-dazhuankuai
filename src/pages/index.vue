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
        actions: {},
        keydowns: {},
      };

      document.addEventListener("keydown", (e) => {
        g.keydowns[e.key] = true;
      });

      document.addEventListener("keyup", (e) => {
        g.keydowns[e.key] = false;
      });

      // 订阅
      g.registerActions = (key, callback) => {
        g.actions[key] = callback;
      };

      setInterval(() => {
        // 发布
        const actions = Object.keys(g.actions);
        for (let i = 0; i < actions.length; i++) {
          const key = actions[i];
          if (g.keydowns[key]) {
            g.actions[key]();
          }
        }

        // 更新
        // g.update();

        //渲染
        g.draw();
      }, 1000 / 30);

      return g;
    };

    const _main = () => {
      const paddle = Paddle();

      const game = GameCanvas();

      let toLeft = false;
      let toRight = false;
      game.ctx.fillStyle = "orange";
      game.ctx.fillRect(paddle.x, 320, 100, 15);

      game.registerActions("s", () => {
        paddle.toLeft();
      });

      game.registerActions("d", () => {
        paddle.toRight();
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