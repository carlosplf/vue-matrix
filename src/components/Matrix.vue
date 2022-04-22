<script setup>
    import Letter from '@/components/Letter.vue'
</script>

<template>
    <div class="container">
        <canvas id="appCanvas" width="1000" height="1000">
            <Letter
                v-for="l in letters"
                :context="vueCtx"
                :ini_char="l.char"
                :ini_position_x="l.position_x"
                :ini_position_y="l.position_y"
            />
        </canvas>
    </div>
</template>

<script>

    export default {
        name: "Matrix",
        props: {},
        data() {
            return{
                letters: [],
                number_of_letters: 200,
                vueCtx: null,
                windowWidth: window.innerWidth,
                windowHeight: window.innerHeight,
            }
        },
        
        created() {
            this.generate_letters();
        },

        components() {
            Letters
        },

        mounted() {
            var c = document.getElementById("appCanvas");
            var ctx = c.getContext("2d");
            ctx.canvas.width  = window.innerWidth;
            ctx.canvas.height = window.innerHeight;
            this.vueCtx = ctx;
            this.animate();
        },

        methods: {
            generate_letters() {
                for(let i=0; i<this.number_of_letters; i++){
                    this.letters = this.letters.concat(
                        {
                            char: Math.random().toString(36).substring(2,3),
                            // Fixed the Y position. Top of page.
                            position_y: 50,
                            // Dynamic X position, filling the line.
                            position_x: parseInt(i*20),
                        }
                    );
                }
            },

            // Apply a dark filter to canvas, making old letters darker.
            async animate() {
                do{
                    this.vueCtx.fillStyle = 'rgba(0,0,0,0.05)';
                    this.vueCtx.fillRect(0,0, this.windowWidth, this.windowHeight);
                    await new Promise(r => setTimeout(r, 20));
                }while(true);
            }
        }
    }

</script>

<style>
    .container{
        position: absolute;
        width: 100%;
        height: 100%;
        margin-top: -10px;
        margin-left: -10px;
    }

    .line{
        display: flex;
        flex-direction: row;
    }
</style>
