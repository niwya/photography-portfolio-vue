<template>
    <lightgallery
        :settings="{ speed: 150, plugins: plugins }"
        :onInit="onInit"
        :onBeforeSlide="onBeforeSlide"
    >
        <a
            v-for="item in items"
            :key="item.id"
            className="gallery-item"
            :data-src="item.src"
        >
            <img className="img-responsive" :src="item.thumb" />
        </a>
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
.gallery-item {
    margin: 5px;
}
</style>
