<template>
    <div class="instrument">
        <div class="freq">
            <b-form-input type="range" v-model="value" :min="freq-distlt" :max="freq+distlt" step="0.0000001" @input="newValue"></b-form-input>
        </div>
        <div class="play">
             <b-button variant="outline-primary" @click="play">Play</b-button>
        </div> 
    </div>
</template>

<script>
export default {
    name: 'instrument',
    props: {
        freq: { 
            type: Number,
            required: true
        },
        distlt:{
            type: Number,
            required: true,
        },
        distgt:{
            type: Number,
            required: true,
        }
    },
    data: function() {
        return {
            value: 0
        }
    },
    watch: {
        freq: {
            handler(newval) {
                this.value = newval;
                this.$emit("input", parseFloat(this.value)||parseInt(this.value));
            },
            immediate: true
        }
    },
    methods: {
        play(ev) {
            let context = new AudioContext()
            let o = context.createOscillator()
            o.type = "sine" 
            o.frequency.value = this.value;
            let  g = context.createGain()
            o.connect(g)
            g.connect(context.destination)
            o.start()
            setTimeout(
                ()=>{
                    context.close();
                }, 
                1500
            )
        },
        newValue() {
            this.$emit("input", parseFloat(this.value)||parseInt(this.value));
        }
    }
}
</script>

<style scoped>
.instrument {
    display: grid;
    width: 100%;
    grid-template-columns: 80% 20%;
}
.instrument > div {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 10px;
}
</style>