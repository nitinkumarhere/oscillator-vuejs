<template>
    <div class="body">
        <div class="application">
            <div>
                <h1> Creating an Interface </h1>
                <p>In this chapter we will go over HTML and CSS</p>
            </div>
            <div class="osc-controls">
                <form>
                    <input id="on-off" type="button" value="start" v-on:click="start">
                    <span>Click to start oscillator</span>
                    <p>Use slider to modify frequency</p>
                    <input id="freq-slider" type= "range" min="0" max="100" step="1" v-model="freqSliderVal">
                </form>
            </div>
            <div class="osc-controls">
                <h2>Waveform</h2>

                <ul id="oscillator-list">   
                    <li id="sawtooth">sawtooth</li>
                    <li id="sine"> sine</li>
                    <li id="triangle">triangle</li>
                    <li id="square">square</li>
                </ul>
            </div>
            <div>
                <p>JavaScript for Sound Artists Demo</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Oscillator',
    msg: String,
    data() {
        return {
            audioContext : new AudioContext(),
            freqSliderVal : Number, // document.getElementById("freq-slider").value,
            osc : false,
            selectedWaveform : "sine",
            // onOff : document.getElementById("on-off"),
            span : document.getElementsByTagName("span")[0],
            waveformTypes : document.getElementsByTagName('li')
        }
    },
    // created() {
    //     var osc = false
    //     var audioContext =  new AudioContext()
    //     var freqSliderVal = document.getElementById("freq-slider").value
    //     var selectedWaveform = "sawtooth";
    //     var waveformTypes = document.getElementsByTagName('li');
    //     var onOff = document.getElementById("on-off");
    //     var span = document.getElementsByTagName("span")[0];
    // },
    
    watch: {
        freqSliderVal : function() {                  // triigers this code when freqSlider value changes.     
            if (!this.osc) {
            console.log("Oscillator is stopped. Waiting for oscillator to start");
            } else {
            this.osc.frequency.value = this.freqSliderVal*10;
            console.log("Oscillator is playing. Frequency value is " +
                this.freqSliderVal*10);
            }
            return 
        },
        selectedWaveform : function() {
            console.log("watch", this.selectedWaveform)
            if (!this.osc) {
            console.log("Oscillator is stopped. Waiting for oscillator to start");
            } else {
                this.osc.type = this.selectedWaveform
            }
        }

    },
    
    methods: {
        start: function() {
            if(!this.osc) {
                console.log("osc", this.osc)
                this.osc = this.audioContext.createOscillator()
                this.osc.frequency.value = this.freqSliderVal*10
                console.log(this.freqSliderVal)
                this.osc.connect(this.audioContext.destination)
                this.osc.start(this.audioContext.currentTime)
                this.onOff.value = "stop"
                this.span.innerHTML = "Click to stop oscillator"
            } else {
                console.log(this.osc)
                this.osc.stop(this.audioContext.currentTime)
                this.osc = false
                this.onOff.value = "start"
                this.span.innerHTML = "Click to start oscillator"
            }
        },
        
        select : function (event) {
            console.log(event.target.id)
            console.log(event)
            this.selectedWaveform = event.target.id //document.getElementById(event.target.id).id
            console.log(this.selectedWaveform)
            this.selectedWaveformElement = document.getElementById(event.target.id);

            for (var i = 0; i < this.waveformTypes.length; i += 1) {
                this.waveformTypes[i].classList.remove("selected-waveform");
            }
              /*_____________________________________END remove any previously
              added selected-waveform classes*/
              /*_____________________________________BEGIN add the selected-
              waveform class to the selected element*/
            this.selectedWaveformElement.classList.add("selected-waveform");
              /*_____________________________________END add the selected-
              waveform class to the selected element*/
            console.log(this.selectedWaveform); // Outputs id
            
        },

    },

    mounted : function() {
        // this.audioContext = new AudioContext()
        this.onOff = document.getElementById("on-off");
        // console.log(this.onOff)
        this.span = document.getElementsByTagName("span")[0];
        // this.osc = false;
        this.waveformTypes = document.getElementsByTagName('li');
        this.freqSliderVal = document.getElementById("freq-slider").value



        for (var i = 0; i < this.waveformTypes.length; i++) {
            this.waveformTypes[i].addEventListener('click', this.select, false);
        }
        // setInterval(function() {
        //     if (!this.osc) {
        //     console.log("Oscillator is stopped. Waiting for oscillator to start");
        //     } else {
        //     this.osc.frequency.value = this.freqSliderVal*10;
        //     this.osc.type = this.selectedWaveform;
        //     console.log("Oscillator is playing. Frequency value is " +
        //         this.freqSliderVal*10);
        //     }
        // }, 500);

        // console.log(this.freqSliderVal);
        // this.onOff.addEventListener("click", function() {
        //     if(!this.osc) {
        //         console.log("osc", this.osc)
        //         this.osc = this.audioContext.createOscillator();

        //         this.osc.frequency.value = this.freqSliderVal*10;
        //         this.osc.connect(this.audioContext.destination);
        //         this.osc.start(this.audioContext.currentTime);
        //         this.onOff.value = "stop";
        //         this.span.innerHTML = "Click to stop oscillator";
        //     } else {
        //         console.log(this.osc)
        //         this.osc.stop(this.audioContext.currentTime);
        //         this.osc = false;
        //         this.onOff.value = "start";
        //         this.span.innerHTML = "Click to start oscillator";
        //     }
        // });
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.body{
    background-color:orange;
    font-family: "Arial";
    }

div{

    padding:20px;

    }

p,li,h1{
    color:green; 
    }

div li{
    color:blue;
    padding:20px;
    }

ul > li{

    color:black;
    /* do something */
    }

#controls{
    color: black;
    /* properties and values go here */
    }

#sawtooth{
    background-color: #336E91;
    }
    #sine{
    background-color: #783d47;
    }
    #triangle{
    background-color: #3b3040;
    }
    #square{
    background-color:    #b85635;
    }

.osc-controls{ /* Notice the dot selector */
    /* set property values */
    border-style:solid;
    border-color: #BC6527;
    border-width: 2px;
    border-radius:10px;
    border-style:solid;
    border-color: #BC6527;
    border-width: 2px;
    border-radius:10px;
    margin-bottom:20px;
    margin-left: 10px;
    margin-right: 10px;
    }

#oscillator-list li{ /* Descendent selector */
    list-style-type: none;
    color:white;
    }

#oscillator-list{
    padding-left:0px;
    }

p,span,li,input{
    font-size:1.5em;
    }

.application{
    width:550px;
    margin:0 auto;
    }

.selected-waveform{
    outline:2px solid white;
    }
</style>
