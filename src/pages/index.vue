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
      //   draw();
    });

    function draw() {
      var ctx = document.getElementById("canvas").getContext("2d");
      for (var i = 0; i < 6; i++) {
        for (var j = 0; j < 6; j++) {
          ctx.fillStyle =
            "rgb(" +
            Math.floor(255 - 42.5 * i) +
            "," +
            Math.floor(255 - 42.5 * j) +
            ",0)";
          ctx.fillRect(j * 25, i * 25, 25, 25);
        }
      }
    }

    // const graph = (ctx)=>{
    //      ctx.fillStyle = "orange";
    //     ctx.fillRect(paddle.x, 320, 100, 15);
    // }

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

    const Ball = () => {
      const obj = {
        x: 150,
        y: 300,
        fired: false,
        speedX: 10,
        speedY: 10,
      };

      obj.fire = () => {
        obj.fired = true;
      };

       // 球移动 
      obj.move = () => {
        if (obj.fired) {
          if (obj.x < 0 || obj.x > 400) {
            obj.speedX = -obj.speedX;
          }
          if (obj.y < 0 || obj.y > 400) {
            obj.speedY = -obj.speedY;
          }
          obj.x += obj.speedX;
          obj.y += obj.speedY;
        }
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
        g.update();

        //渲染
        g.draw();
      }, 1000 / 30);

      return g;
    };

    const _main = () => {
      const paddle = Paddle();

      const game = GameCanvas();

      const ball = Ball();

      //   let toLeft = false;
      //   let toRight = false;
      const ctx = game.ctx;
      ctx.fillStyle = "orange";
      ctx.fillRect(paddle.x, 320, 100, 15);

      ctx.fillStyle = "green";
      ctx.fillRect(ball.x, ball.y, 25, 25);

      // 注册按键
      game.registerActions("s", () => {
        paddle.toLeft();
      });

      game.registerActions("d", () => {
        paddle.toRight();
      });

      game.registerActions("f", () => {
        ball.fire();
      });

      game.update = () => {
        ball.move();
      };

      game.draw = () => {
        ctx.clearRect(0, 0, game.canvas.width, game.canvas.height); // 清除整个画布
        ctx.fillStyle = "orange";
        ctx.fillRect(paddle.x, 320, 100, 15);

        ctx.fillStyle = "green";
        ctx.fillRect(ball.x, ball.y, 25, 25);
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