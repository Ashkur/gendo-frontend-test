<template>
  <content-loader
    :width="150"
    :height="460"
    :speed="2"
    primaryColor="#f3f3f3"
    secondaryColor="#ecebeb"
  >
    <circle cx="15" cy="32" r="15" />
    <rect x="33" y="23" rx="2" ry="2" width="64" height="5" />
    <rect x="34" y="35" rx="2" ry="2" width="97" height="7" />
    <rect x="3" y="62" rx="0" ry="0" width="135" height="10" />
    <rect x="4" y="79" rx="0" ry="0" width="115" height="6" />
    <rect x="53" y="88" rx="0" ry="0" width="0" height="1" />
    <rect x="5" y="91" rx="0" ry="0" width="110" height="6" />
    <rect x="7" y="113" rx="0" ry="0" width="135" height="10" />
    <rect x="8" y="130" rx="0" ry="0" width="115" height="6" />
    <rect x="57" y="139" rx="0" ry="0" width="0" height="1" />
    <rect x="9" y="142" rx="0" ry="0" width="110" height="6" />
    <rect x="6" y="163" rx="0" ry="0" width="135" height="10" />
    <rect x="7" y="180" rx="0" ry="0" width="115" height="6" />
    <rect x="56" y="189" rx="0" ry="0" width="0" height="1" />
    <rect x="8" y="192" rx="0" ry="0" width="110" height="6" />
  </content-loader>
</template>

<script>
import { ContentLoader } from "vue-content-loader";

export default {
  components: {
    ContentLoader,
  },
};
</script>
