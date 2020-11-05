<template>
    <div id="app">
        <h1>Welcome to Christopher Chua's Colour Coding app</h1>
        <h3>A 1280x640 canvas containing 32,768 5px squares, each filled with discrete colours</h3>
        <h4>Choose one of the following randomly generated designs</h4>

        <button v-on:click="show('wave')">"Wave from Randomness"</button>
        <button v-on:click="show('silicon')">"Silicon Wafers"</button>
        
        <image-canvas v-if="showImage" v-bind:imageType="imageType" :key="key"></image-canvas>
    </div>
</template>

<script>
import ImageCanvas from './components/ImageCanvas.vue';

export default {
    name: 'App',
    components: {
        'image-canvas': ImageCanvas
    },
    data() {
        return {
            imageType: null, // null on first load
            showImage: false, // false on first load
            key: 0 // our image-canvas component is binded to this key, any changes to the key will force Vue to refresh that component
        }
    },
    methods: {
        show(imageType) {
            this.showImage = true; // user has clicked either button for the first time, now we show an image-canvas
            this.imageType = imageType; // captured the intended image type, now it'll be passed as a prop to the image-canvas component
            this.key++; // update the key to force Vue to refresh the image-canvas component
        }
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    margin-top: 30px;
}

button {
    font-size: 16px;
}

canvas {
    margin-top: 15px;
}
</style>
