<template>
    <div>
        <canvas width="1280" height="640"></canvas>
    </div>
</template>

<script>
export default {
    name: 'ImageCanvas',
    props: {
        imageType: String
    },
    data() {
        return {
            canvas: null, // to hold our canvas element
            ctx: null, // to hold our canvas context
            colours: [] // to hold our array of rgb colour strings
        }
    },
    mounted() {
        this.canvas = document.querySelector('canvas');
        this.ctx = this.canvas.getContext('2d');

        this.populateColours(); // first populate our colours array with 32,768 discrete colours
        this.drawImage(this.imageType); // depending on the selected image type, fill the coloured squares a certain way
    },
    methods: {
        populateColours() {
            // populate an array to hold 32,768 rgb colour strings, from 'rgb(8, 8, 8)' to 'rgb(256, 256, 256)'
            for (let i = 1; i <= 32; i++) { // ignoring 0 values, we have 32 steps from 8..256
                for (let j = 1; j <= 32; j++) {
                    for (let k = 1; k <= 32; k++) {
                        this.colours.push(`rgb(${k * 8}, ${j * 8}, ${i * 8})`);
                    }
                }
            }
        },
        drawImage(imageType) {
            if (imageType === 'silicon') {
                let index = this.getRandomIndex(this.colours); // get a random index to start with
                for (let row = 1; row <= 256; row++) { // our image will be 256 squares along the x axis
                    for (let column = 1; column <= 128; column++) { // our image will be 128 squares along the y axis
                        if (index < 0) { // if index is out of bounds
                            index = this.getRandomIndex(this.colours); // reset the index back into somewhere in the array
                        }

                        this.ctx.fillStyle = this.colours[index]; // fill this square with the colour at this index of our colours array
                        this.ctx.fillRect(row * 5, column * 5, 5, 5);

                        this.colours.splice(index, 1); // remove the colour we just used from the array, so we won't ever use it again

                        index--; // decrement index (heading to zero)
                    }
                }
            } else if (imageType === 'wave') {
                for (let row = 1; row <= 256; row++) { // our image will be 256 squares along the x axis
                    for (let column = 1; column <= 128; column++) { // our image will be 128 squares along the y axis
                        let rand = Math.floor(Math.random() * this.colours.length / row); // get a random index but now divide it by the row # we're at

                        this.ctx.fillStyle = this.colours[rand]; // fill this square with the colour at this rand index of our colours array
                        this.ctx.fillRect(row * 5, column * 5, 5, 5);

                        this.colours.splice(rand, 1); // remove the colour we just used from the array, so we won't ever use it again
                    }
                }
            }
        },
        getRandomIndex(array) {
            return Math.floor(Math.random() * array.length); // find a random index that will be inside the array
        }
    }
}
</script>

<style scoped>

</style>
