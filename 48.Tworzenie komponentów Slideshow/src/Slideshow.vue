<template>

    <div class="slides-wrapper" :class="{'loading':isLoading}">

        <button class="btn btn-primary btn-action btn-lg slides-prev" :disabled="start || isLoading" @click="changeSlide(-1)">
            <i class="icon icon-arrow-left"></i>
        </button>

        <div class="slides">
            <slide :url="activeUrl" :number="number" :class="{'dinmed':isLoading}" />
        </div>

        <button class="btn btn-primary btn-action btn-lg slides-next" :disabled="end || isLoading" @click="changeSlide(1)">
            <i class="icon icon-arrow-right"></i>
        </button>

    </div>

</template>

<script>

    import {preloadingImage} from "../helpers.js/helpers.js";

    import Slide from "./Slide";

    export default {
        name: "Slideshow",
        props: {
            images: Array
        },
        data() {
            return {
                active: 0,
                isLoading:false
            }
        },
        computed: {
            activeUrl() {
                return this.images[this.active].url
            },
            number() {
                return `${this.active + 1}/${this.images.length}`;
            },
            start() {
                return this.active === 0;
            },
            end() {
                return this.active === this.images.length - 1;
            }
        },
        methods: {
            changeSlide(dir) {

                let index = this.active + dir;
                let slide = this.images[index];

                if(slide !== undefined) {
                    this.isLoading=true;
                   preloadingImage(this.image[index].url)
                    .then(url =>{
                        this.active=index;
                        this.isLoading=false;
                    });
                    
                }

            }
        },
        components: {
            Slide
        }
    };

</script>

<style scoped>

    .slides-wrapper {
        min-height: 300px;
        position: relative;
    }

    .slides-next,
    .slides-prev {
        position: absolute;
        top: 50%;

        transform: translateY(-50%);
    }

    .slides-prev {
        left: 0;
    }

    .slides-next {
        right: 0;
    }

</style>
