<template>
  <div>
    <div style="position:relative;" v-if="displayCevher">
      <!-- map component -->
      <vl-map
        :load-tiles-while-animating="true"
        :load-tiles-while-interacting="true"
        data-projection="EPSG:4326"
        style="height: 500px"
      >
        <!-- view component -->
        <vl-view
          ref="view"
          name="view"
          :zoom.sync="zoom"
          :min-zoom="2"
          :max-zoom="20"
          :center.sync="center"
          :rotation.sync="rotation"
        ></vl-view>
        <!--// view component -->

        <!-- base layer -->
        <vl-layer-tile id="bingmaps">
          <vl-source-bingmaps :api-key="apiKey" :imagery-set="baseMapStyle"></vl-source-bingmaps>
        </vl-layer-tile>
        <!--// base layer -->

        <!-- layers component -->
        <component
          v-for="layer in layers"
          :is="layer.cmp"
          v-if="layer.visible"
          :key="layer.id"
          v-bind="layer"
        >
          <!-- source component -->
          <component :is="layer.source.cmp" v-bind="layer.source">
            <component
              v-if="layer.source.source"
              :is="layer.source.source.cmp"
              v-bind="layer.source.source"
            ></component>
          </component>
          <!--// source component -->
        </component>
        <!--// layers component -->
      </vl-map>
      <!--// map component -->

      <!-- layers panel -->
      <div style="position:absolute;top:0px;right:0px;width:200px;">
        <b-collapse class="panel box is-paddingless">
          <div slot="trigger" class="panel-heading">
            <div class="columns">
              <div class="column is-11" style="font-size:16px;">
                <strong>Layers</strong>
              </div>
            </div>
          </div>

          <div style="max-height:200px;overflow:auto;">
            <div style="font-size:13px;margin:5px;" v-for="layer in layers" :key="layer.id">
              <b-switch :key="layer.id" v-model="layer.visible">{{ layer.title }}</b-switch>
            </div>
          </div>
        </b-collapse>
      </div>
      <!--// layers panel -->
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import VueLayers from "vuelayers";
import "vuelayers/lib/style.css";
//import proj4 from "proj4";

Vue.use(VueLayers, {
  dataProjection: "EPSG:4326"
});

export default {
  name: "cevher",
  components: {
    Vue,
    VueLayers
  },
  props: {
    displayCevher: {
      type: Boolean,
      required: true
    },
    apiKey: {
      type: String,
      required: false,
      default: "cevher"
    },
    zoom: {
      type: Number,
      required: false,
      default: 10
    },
    center: {
      type: Array,
      required: false,
      default: () => [28.8724219692539, 41.04775534936306]
    },
    baseMapStyle: {
      type: String,
      required: false,
      default: "Road"
    },
    layers: {
      type: Object
    }
  },
  data() {
    return {
      rotation: 0
    };
  }
};
</script>

<style lang="css">
.test {
  background-color: antiquewhite;
}
</style>