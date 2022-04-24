<script>
    export default{
        name: "Letter",
        
        props: {
            ini_char: String,
            ini_position_x: Number,
            ini_position_y: Number,
            context: {},
        },

        data() {
            return{
                position_x: this.ini_position_x,
                position_y: this.ini_position_y,
                chat: this.ini_char,
                v_speed: 10,
                windowWidth: window.innerWidth,
                windowHeight: window.innerHeight,
                max_x: window.innerWidth - 10,
                max_y: window.innerHeight - 100,
                color_gradient: null,
            }
        },
        
        mounted() {
            this.life_cycle();
        },

        methods: {

            create_color_gradient() {
                
                if(this.context === null || this.color_gradient != null) return -1;

                this.color_gradient = this.context.createLinearGradient(0,0,this.windowWidth, this.windowHeight) 
                this.color_gradient.addColorStop(0, "red");
                this.color_gradient.addColorStop(0.2, "yellow");
                this.color_gradient.addColorStop(0.4, "green");
                this.color_gradient.addColorStop(0.6, "cyan");
                this.color_gradient.addColorStop(0.8, "blue");
                this.color_gradient.addColorStop(1, "magenta");
            },

            // Control the Letter life cycle.
            // Going down on page and back to top.
            async life_cycle() {
                do{
                    this.position_y = parseInt(this.position_y) + this.v_speed;
                    // If letter reach the end of page, go to top.
                    // The random value comparison control the chance to a Letter go back
                    // to top of page.
                    if (this.position_y > this.max_y && Math.random() > 0.98){
                        this.position_y = 0;
                    }

                    this.char = Math.random().toString(36).substring(2,3),
                    
                    // Delay in ms to update a Letter. Position and value.
                    await new Promise(r => setTimeout(r, 20));
                
                }while(true);
            }
        },

        render() {
            if(this.context === null) return null;

            if(this.color_gradient == null) this.create_color_gradient();

            else{
                this.context.font = "20px Monospace";
                this.context.fillStyle = this.color_gradient;
                this.context.fillText(this.char, this.position_x, this.position_y);
            }
        },
    }
</script>

<style scope>
</style>
