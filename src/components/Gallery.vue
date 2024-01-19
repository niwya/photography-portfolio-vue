<template>
    <lightgallery
        class="gallery"
        :settings="gallerySettings"
        :onInit="onInit"
        :onBeforeSlide="onBeforeSlide"
    >
        <div
            v-for="galleryRow in items"
            :key="galleryRow.id"
            class="gallery-row"
        >
            <a
                v-for="(galleryItem, itemIndex) in galleryRow"
                :data-src="galleryItem.src"
                :key="itemIndex"
                class="gallery-item"
                :style="`--r: ${galleryItem.ratio}`"
                :data-lg-size="galleryItem.size"
            >
                <img
                    className="img-responsive"
                    :src="galleryItem.src"
                    @load="updateRatio($event, galleryItem)"
                />
            </a>
        </div>
    </lightgallery>
</template>

<script>
import Lightgallery from "lightgallery/vue";
import lgZoom from "lightgallery/plugins/zoom";
import lgHash from "lightgallery/plugins/hash";
import lgFullscreen from "lightgallery/plugins/fullscreen";
import lgAutoplay from "lightgallery/plugins/autoplay";
import lgThumbnail from "lightgallery/plugins/thumbnail";
// import lgPager from "lightgallery/plugins/pager";

import galeryItems from "@/assets/gallery-items.json";

let lightGallery = null;

export default {
    name: "App",
    components: {
        Lightgallery,
    },
    data: () => ({
        items: galeryItems,
        gallerySettings: {
            speed: 350,
            plugins: [lgHash, lgZoom, lgFullscreen, lgAutoplay, lgThumbnail], // lgPager
            selector: ".gallery-item",
            mode: "lg-slide-skew",
            mousewheel: true,
        },
    }),
    methods: {
        onInit: (detail) => {
            lightGallery = detail.instance;
            console.log("LightGallery initialized", lightGallery);
        },
        onBeforeSlide: () => {
            console.log("calling before slide");
        },
        updateRatio(event, galleryItem) {
            const img = event.target;
            galleryItem.ratio = img.naturalWidth / img.naturalHeight;
        },
    },
};
</script>
<style lang="css">
@import "lightgallery/css/lightgallery.css";
@import "lightgallery/css/lg-zoom.css";
@import "lightgallery/css/lg-fullscreen.css";
@import "lightgallery/css/lg-transitions.css";
@import "lightgallery/css/lg-autoplay.css";
@import "lightgallery/css/lg-thumbnail.css";
/* @import "lightgallery/css/lg-pager.css"; */

body {
    margin: 0;
}

.gallery {
    display: flex;
    flex-direction: column;
}

.gallery .gallery-row {
    display: flex;
    width: 95%;
    max-width: 1200px;
    height: auto;
    margin: 0 auto;

    flex-direction: row;
    @media screen and (max-width: 768px) {
        flex-direction: column;
    }
}

.gallery .gallery-row .gallery-item {
    /* margin: 5px; */
    padding: 15px;
    flex-grow: calc(var(--r));
    flex-basis: 0;
}

.gallery-item img {
    max-width: 100%;
    box-shadow: 0 10px 20px -4px rgba(0, 0, 0, 0.6);
}
</style>
