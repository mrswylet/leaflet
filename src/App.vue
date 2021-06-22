<template>
  <div id="app">
    <div
        ref="map"
        id="map"></div>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import L from 'leaflet';

import 'leaflet.vectorgrid/dist/Leaflet.VectorGrid.bundled.min'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      map: null,
      vectorTileOptions: {
        vectorTileLayerStyles: {
          "Кадастровые округа":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые округа/label":{stroke:!1,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые округа:1":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые округа:1/label":{stroke:!1,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые округа:2":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые округа:2/label":{stroke:!1,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые районы":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые районы/label":{stroke:!1,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые районы:1":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые районы:1/label":{stroke:!1,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые районы:2":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые районы:2/label":{stroke:!1,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые кварталы":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Кадастровые кварталы/label":{stroke:!1,color:"#D20404",opacity:0.5,weight:1},
          "Здания_area":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Здания":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Кварталы_all":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Дороги":{stroke:!1,color:"#D20404",opacity:0.50,weight:1},
          "Дороги_1m":{stroke:!1,color:"#D20404",opacity:0.50,weight:1},
          "Жд станции":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Железные дороги":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Парки":{stroke:!1,color:"#D20404",opacity:0.50,weight:1},
          "Реки (полигоны)":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Озера (полигоны)_all":{stroke:!1,color:"#D20404",opacity:0.50,weight:1},
          "Озера (полигоны)_area":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Острова":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Растительность_1mln ":{stroke:!1,color:"#D20404",opacity:0.50,weight:1},
          "Гидрография линии:2":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Гидрография линии:1":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Здания_3d":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Населенные пункты (полигоны)":{stroke:!0,color:"#D20404",opacity:0.5,weight:1},
          "Кварталы_AREA_100":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
          "Кварталы_AREA_500":{stroke:!0,color:"#D20404",opacity:0.50,weight:1},
        },
        minNativeZoom: 4,
      }
    }
  },
  mounted() {


    const { map } = this.$refs;
    this.map = L.map(map, {
      center: [56.654433, 60.606765],
      zoom: 16
    });


    const ymapLayer = L.tileLayer(
        'http://vec{s}.maps.yandex.net/tiles?l=map&v=4.55.2&z={z}&x={x}&y={y}&scale=2&lang=ru_RU', {
          subdomains: ['01', '02', '03', '04'],
          attribution: '<a http="yandex.ru" target="_blank">Яндекс</a>',
          reuseTiles: true,
          updateWhenIdle: false
        }
    );
    ymapLayer.addTo(this.map)

    // eslint-disable-next-line no-unused-vars
    const esriTilesUrl = "https://pkk.rosreestr.ru/arcgis/rest/services/Hosted/caddivsion/VectorTileServer/tile/{z}/{y}/{x}.pbf";
    // eslint-disable-next-line no-unused-vars
    const baseMapTileUrl =  "https://pkk.rosreestr.ru/arcgis/rest/services/Hosted/BASEMAP_OSM/VectorTileServer/tile/{z}/{y}/{x}.pbf"
    // eslint-disable-next-line no-unused-vars
    const esriTilesUrlBig = "https://pkk.rosreestr.ru/arcgis/rest/services/Hosted/vt_anno_light/VectorTileServer/tile/{z}/{y}/{x}.pbf";
    const resreestrLayer = L.vectorGrid.protobuf(baseMapTileUrl, this.baseMapTileUrl);
    resreestrLayer.addTo(this.map)
    this.map.on('click', () => {
      console.log(this.map.getPanes())
      this.map.hasLayer(ymapLayer)
          ? this.map.removeLayer(ymapLayer)
          : this.map.addLayer(ymapLayer);
    });

    const popup = L.popup()
        .setLatLng([56.654433, 60.606765])
        .setContent("I am a standalone popup.")
        .openOn(this.map);

    this.map.on('click', (e) => {
      const { lat, lng } = e.latlng
      popup
          .setLatLng([lat, lng])
          .setContent(`I am a standalone popup. ${lat} - ${lng}`)
          .openOn(this.map)
    });

    // const baseMapTileUrl =  "https://pkk.rosreestr.ru/arcgis/rest/services/Hosted/BASEMAP_OSM/VectorTileServer/tile/{z}/{y}/{x}.pbf"
    // const esriTilesPbfBMLayer = new L.VectorGrid.Protobuf(baseMapTileUrl, vectorTileOptions);
    //
    // const esriTilesUrlBig = "https://pkk.rosreestr.ru/arcgis/rest/services/Hosted/vt_anno_light/VectorTileServer/tile/{z}/{y}/{x}.pbf";
    // const esriTilesPbfBigLayer = new L.VectorGrid.Protobuf(esriTilesUrlBig, vectorTileOptions);
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#map {
  height: 500px;
  overflow: hidden;
  background-color: #c2c2c2;
}
</style>
