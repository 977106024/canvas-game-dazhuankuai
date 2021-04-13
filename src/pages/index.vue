<template>
    <div>
        <canvas ref="game" id="canvas" width="400" height="400"></canvas>
    </div>
</template>

<script>
import { onMounted, reactive, ref } from 'vue'
export default {
    setup() {
        const v = ref('这是一个字符串')
        const o = reactive({a:1})
        console.log(Reflect.get(o,'a'),'0')

        const game = ref(null)

        onMounted(()=>{
             const obj = {
                x:150,
                y:320
            }
            const canvas = game.value
            const ctx = canvas.getContext('2d')
            ctx.fillStyle = 'orange'
            ctx.fillRect(obj.x,320,100,15)

            document.addEventListener('keydown',(e)=>{
                const key = e.key
                 ctx.clearRect(obj.x,320,100,15)
                if(key === 'f'){
                    obj.x-=50
                }else if(key === 'j'){
                    obj.x+=50
                }
                ctx.fillRect(obj.x,320,100,15)

              
            })
        })

        

        
        

        return {v, game}
    },
}
</script>

<style lang="scss" scoped>
    canvas{
        border: 1px solid #ccc;
    }
</style>