<template>
    <lightgallery
        class="gallery"
        :settings="gallerySettings"
        :onInit="onInit"
        :onBeforeSlide="onBeforeSlide"
    >
        <div>
            <h1>Light Gallery</h1>

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
                >
                    <img
                        className="img-responsive"
                        :src="galleryItem.src"
                        @load="updateRatio($event, galleryItem)"
                    />
                </a>
            </div>
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
    data: () => ({
        items: galeryItems,
        gallerySettings: {
            speed: 150,
            plugins: [lgZoom],
            selector: ".gallery-item",
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
    flex-grow: calc(var(--r));
    flex-basis: 0;
}

.gallery-item img {
    max-width: 100%;
}
</style>
