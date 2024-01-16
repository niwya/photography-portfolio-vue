<template>
    <lightgallery
        class="gallery"
        :settings="{ speed: 150, plugins: plugins }"
        :onInit="onInit"
        :onBeforeSlide="onBeforeSlide"
    >
        <div
            v-for="(galleryRow, index) in items"
            :key="index"
            class="gallery-row"
        >
            <a
                v-for="galleryItem in galleryRow"
                :key="galleryItem.id"
                className="gallery-item"
                :data-src="galleryItem.src"
            >
                <img className="img-responsive" :src="galleryItem.src" />
            </a>
        </div>
    </lightgallery>
</template>

<script>
import Lightgallery from "lightgallery/vue";
import lgZoom from "lightgallery/plugins/zoom";

import galeryItems from "@/assets/gallery-items.json";

let lightGallery = null;
export default {
    name: "App",
    components: {
        Lightgallery,
    },
    watch: {
        items(newVal, oldVal) {
            this.$nextTick(() => {
                lightGallery.refresh();
            });
        },
    },
    data: () => ({
        plugins: [lgZoom],
        items: galeryItems,
    }),
    methods: {
        onInit: (detail) => {
            lightGallery = detail.instance;
        },
        onBeforeSlide: () => {
            console.log("calling before slide");
        },
    },
};
</script>
<style lang="css">
@import url("https://cdn.jsdelivr.net/npm/lightgallery@2.1.0-beta.1/css/lightgallery.css");
@import url("https://cdn.jsdelivr.net/npm/lightgallery@2.1.0-beta.1/css/lg-zoom.css");

body {
    margin: 0;
}

.gallery {
    display: flex;
    flex-direction: column;
}

.gallery .gallery-row {
    display: flex;
    flex-direction: row;
    width: 95%;
    max-width: 1200px;
    height: auto;
    margin: 0 auto;
}

.gallery .gallery-row .gallery-item {
    margin: 5px;
}

.gallery img {
    width: 100%;
}
</style>
