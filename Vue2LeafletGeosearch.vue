<template>
  <div></div>
</template>

<script>
import {
  GeoSearchControl,
  OpenStreetMapProvider,
  GoogleProvider,
} from "leaflet-geosearch";

export default {
  props: ["apiKey"],
  name: "v-geosearch",
  mounted() {
    this.add();
  },
  beforeDestroy() {
    this.remove();
  },
  methods: {
    deferredMountedTo(parent) {
      const provider = new GoogleProvider({
        params: {
          key: this.apiKey,
        },
      });
      const searchControl = new GeoSearchControl({ provider: provider });
      parent.addControl(searchControl);
      searchControl.getContainer().onclick = (e) => {
        e.stopPropagation();
      };
    },
    remove() {
      if (this.markerCluster) {
        this.$parent.removeLayer(this.markerCluster);
      }
    },
    add() {
      if (this.$parent._isMounted) {
        this.deferredMountedTo(this.$parent.mapObject);
      }
    },
  },
};
</script>
