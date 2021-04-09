<template>
  <div id="map"></div>
</template>

<script>
import 'leaflet/dist/leaflet.css'

import L from 'leaflet';
import markerIcon from 'leaflet/dist/images/marker-icon.png'
import markerIcon2x from 'leaflet/dist/images/marker-icon-2x.png'
import markerShadow from 'leaflet/dist/images/marker-shadow.png'
import { find } from 'loadsh';
import axios from 'axios';

export default {
  data() {
    return {
      map: null,
      info: null,
      center: [24.487713, 118.129859],
      tk: "2703f0a1405a588d96232ce29cfa75ca"
    }
  },
  created() {
    let DefaultIcon = L.icon({
      iconUrl: markerIcon,
      iconRetinaUrl: markerIcon2x,
      shadowUrl: markerShadow,
      iconSize: [25, 41],
      iconAnchor: [12, 41],
      popupAnchor: [1, -34],
      tooltipAnchor: [16, -28],
      shadowSize: [41, 41]
    });
    L.Marker.prototype.options.icon = DefaultIcon;
  },
  mounted() {
    this.init();
    this.initLayer();
    this.initControl();
    this.initControl2();
  },
  methods: {
    init() {
      let self = this;
      this.map = L.map('map');
      this.map.setView([24.529594499216834, 118.16049098968507], 17);

      function getColor(d) {
        return d > 1000 ? '#800026' :
          d > 500 ? '#BD0026' :
            d > 200 ? '#E31A1C' :
              d > 100 ? '#FC4E2A' :
                d > 50 ? '#FD8D3C' :
                  d > 20 ? '#FEB24C' :
                    d > 10 ? '#FED976' :
                      '#FFEDA0';
      }
      function style(feature) {
        return {
          fillColor: getColor(feature.properties.density),
          weight: 2,
          opacity: 1,
          color: 'white',
          dashArray: '3',
          fillOpacity: 0.7
        }
      }
      function highlightFeature(e) {
        var layer = e.target;
        layer.setStyle({
          weight: 5,
          color: '#666',
          dashArray: '',
          fillOpacity: 0.7
        });
        if (!L.Browser.ie && !L.Browser.opera && !L.Browser.edge) {
          layer.bringToFront();
        }
        self.info.update(layer.feature.properties);
      }
      function resetHighlight(e) {
        self.geojson.resetStyle(e.target);
        self.info.update();
      }
      function zoomToFeature(e) {
        this.map.fitBounds(e.target.getBounds());
      }
      function onEachFeature(feature, layer) {
        layer.on({
          mouseover: highlightFeature,
          mouseout: resetHighlight,
          click: zoomToFeature
        });
      }
      let process = resp => {
        self.geojson = L.geoJSON(resp.data, {
          style: style,
          onEachFeature: onEachFeature
        }).addTo(this.map);
      }
      axios.get("http://172.22.241.110:8080/geoserver/zhcq/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=zhcq%3Abuildings&maxFeatures=50&outputFormat=application%2Fjson").then(process);
    },
    initLayer() {
      L.tileLayer('http://t{s}.tianditu.gov.cn/img_w/wmts?tk=' + this.tk + '&SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}', {
        subdomains: [0, 1, 2, 3, 4, 5, 6, 7],
        opacity: 0.9
      }).addTo(this.map);
    },
    initLayerGeoJson() {
      let getColor = data => {
        return "red";
      }
      // style，自定义风格
      let style = feature => {
        return {
          fillColor: getColor(feature.properties.density),
          weight: 2,
          opacity: 1,
          color: 'white',
          dashArray: '3',
          fillOpacity: 0.7
        };
      }
      // style，自定义风格
      let filter = (feature, layer) => {
        return true;
      }
      // onEachFeature，遍历feature时执行
      let onEachFeature = (feature, layer) => {
        layer.bindPopup(feature.properties.code);
      }

      let process = resp => {
        L.geoJSON(resp.data, {
          style: style,
          filter: filter,
          onEachFeature: onEachFeature
        }).addTo(this.map);
      }

      axios.get("http://172.22.241.110:8080/geoserver/zhcq/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=zhcq%3Abuildings&maxFeatures=50&outputFormat=application%2Fjson").then(process);
    },
    initControl() {
      this.info = L.control();
      this.info.onAdd = function (map) {
        this._div = L.DomUtil.create('div', 'info'); // create a div with a class "info"
        this.update();
        return this._div;
      };
      // method that we will use to update the control based on feature properties passed
      this.info.update = function (props) {
        this._div.innerHTML = '<h4>US Population Density</h4>' + (props ?
          '<b>' + props.name + '</b><br />' + props.density + ' people / mi<sup>2</sup>'
          : 'Hover over a state');
      };
      this.info.addTo(this.map);
    },
    initControl2() {
      let getColor = function (d) {
        return d > 1000 ? '#800026' :
          d > 500 ? '#BD0026' :
            d > 200 ? '#E31A1C' :
              d > 100 ? '#FC4E2A' :
                d > 50 ? '#FD8D3C' :
                  d > 20 ? '#FEB24C' :
                    d > 10 ? '#FED976' :
                      '#FFEDA0';
      }
      var legend = L.control({ position: 'bottomright' });
      legend.onAdd = function (map) {
        var div = L.DomUtil.create('div', 'info legend'),
          grades = [0, 10, 20, 50, 100, 200, 500, 1000],
          labels = [];
        // loop through our density intervals and generate a label with a colored square for each interval
        for (var i = 0; i < grades.length; i++) {
          div.innerHTML +=
            '<i style="background:' + getColor(grades[i] + 1) + '"></i> ' +
            grades[i] + (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+');
        }
        return div;
      };
      legend.addTo(this.map);

    }
  }
}
</script>

<style>
#map {
  height: 700px;
  width: 100%;
  padding: 0;
  margin: 0;
  background-color: black;
}

.info {
  padding: 6px 8px;
  font: 14px/16px Arial, Helvetica, sans-serif;
  background: white;
  background: rgba(255, 255, 255, 0.8);
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
}
.info h4 {
  margin: 0 0 5px;
  color: #777;
}

.legend {
  line-height: 18px;
  color: #555;
}
.legend i {
  width: 18px;
  height: 18px;
  float: left;
  margin-right: 8px;
  opacity: 0.7;
}
</style>
