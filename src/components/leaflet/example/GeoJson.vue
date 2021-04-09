<template>
  <div id="map"></div>
</template>

<script>
import 'leaflet/dist/leaflet.css'

// import { Form, Loading } from 'element-ui'
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
    this.initLayerGeoJson();
  },
  methods: {

    init() {
      var builds = L.tileLayer.wms("http://172.22.241.110:8080/geoserver/zhcq/wms?service=WMS&version=1.1.0&request=GetMap&layers=zhcq%3Abuildings&bbox=73.62%2C16.7%2C134.77%2C53.56&width=768&height=462&srs=EPSG%3A4490&format=application%2Fopenlayers3", {
      });
      //地图对象
      this.map = L.map('map', {
        // Options
        // Boolean false 
        preferCanvas: false,
        // Control options
        // attributionControl	Boolean	true	Whether a attribution control is added to the map by default.
        // zoomControl	Boolean	true	Whether a zoom control is added to the map by default.
        // Interaction Options
        // closePopupOnClick	Boolean	true	Set it to false if you don't want popups to close when user clicks the map.
        // zoomSnap	Number	1	Forces the map's zoom level to always be a multiple of this, particularly right after a fitBounds() or a pinch-zoom. By default, the zoom level snaps to the nearest integer; lower values (e.g. 0.5 or 0.1) allow for greater granularity. A value of 0 means the zoom level will not be snapped after fitBounds or a pinch-zoom.
        // zoomDelta	Number	1	Controls how much the map's zoom level will change after a zoomIn(), zoomOut(), pressing + or - on the keyboard, or using the zoom controls. Values smaller than 1 (e.g. 0.5) allow for greater granularity.
        // trackResize	Boolean	true	Whether the map automatically handles browser window resize to update itself.
        // boxZoom	Boolean	true	Whether the map can be zoomed to a rectangular area specified by dragging the mouse while pressing the shift key.
        // doubleClickZoom	Boolean|String	true	Whether the map can be zoomed in by double clicking on it and zoomed out by double clicking while holding shift. If passed 'center', double-click zoom will zoom to the center of the view regardless of where the mouse was.
        // dragging	Boolean	true	Whether the map be draggable with mouse/touch or not.
        // Map State Options
        // crs	CRS	L.CRS.EPSG3857	The Coordinate Reference System to use. Don't change this if you're not sure what it means.
        // center	LatLng	undefined	Initial geographic center of the map
        // zoom	Number	undefined	Initial map zoom level
        // minZoom	Number	*	Minimum zoom level of the map. If not specified and at least one GridLayer or TileLayer is in the map, the lowest of their minZoom options will be used instead.
        // maxZoom	Number	*	Maximum zoom level of the map. If not specified and at least one GridLayer or TileLayer is in the map, the highest of their maxZoom options will be used instead.
        // layers	Layer[]	[]	Array of layers that will be added to the map initially
        // maxBounds	LatLngBounds	null	When this option is set, the map restricts the view to the given geographical bounds, bouncing the user back if the user tries to pan outside the view. To set the restriction dynamically, use setMaxBounds method.
        // renderer	Renderer	*	The default method for drawing vector layers on the map. L.SVG or L.Canvas by default depending on browser support.

        // Animation Options

        // Panning Inertia Options

        // Keyboard Navigation Options
        // Mouse wheel options
        // Touch interaction options



        // Methods
        // Method	Returns	Description
        // getRenderer(<Path> layer)	Renderer	
        // Returns the instance of Renderer that should be used to render the given Path. It will ensure that the renderer options of the map and paths are respected, and that the renderers do exist on the map.

        // Methods for Layers and Controls
        // Method	Returns	Description
        // addControl(<Control> control)	this	
        // Adds the given control to the map

        // removeControl(<Control> control)	this	
        // Removes the given control from the map

        // addLayer(<Layer> layer)	this	
        // Adds the given layer to the map

        // removeLayer(<Layer> layer)	this	
        // Removes the given layer from the map.

        // hasLayer(<Layer> layer)	Boolean	
        // Returns true if the given layer is currently added to the map

        // eachLayer(<Function> fn, <Object> context?)	this	
        // Iterates over the layers of the map, optionally specifying context of the iterator function.

        // map.eachLayer(function(layer){
        //     layer.bindPopup('Hello');
        // });
        // openPopup(<Popup> popup)	this	
        // Opens the specified popup while closing the previously opened (to make sure only one is opened at one time for usability).

        // openPopup(<String|HTMLElement> content, <LatLng> latlng, <Popup options> options?)	this	
        // Creates a popup with the specified content and options and opens it in the given point on a map.

        // closePopup(<Popup> popup?)	this	
        // Closes the popup previously opened with openPopup (or the given one).

        // openTooltip(<Tooltip> tooltip)	this	
        // Opens the specified tooltip.

        // openTooltip(<String|HTMLElement> content, <LatLng> latlng, <Tooltip options> options?)	this	
        // Creates a tooltip with the specified content and options and open it.

        // closeTooltip(<Tooltip> tooltip?)	this	
        // Closes the tooltip given as parameter.

        // Methods for modifying map state
        // Method	Returns	Description
        // setView(<LatLng> center, <Number> zoom, <Zoom/pan options> options?)	this	
        // Sets the view of the map (geographical center and zoom) with the given animation options.

        // setZoom(<Number> zoom, <Zoom/pan options> options?)	this	
        // Sets the zoom of the map.

        // zoomIn(<Number> delta?, <Zoom options> options?)	this	
        // Increases the zoom of the map by delta (zoomDelta by default).

        // zoomOut(<Number> delta?, <Zoom options> options?)	this	
        // Decreases the zoom of the map by delta (zoomDelta by default).

        // setZoomAround(<LatLng> latlng, <Number> zoom, <Zoom options> options)	this	
        // Zooms the map while keeping a specified geographical point on the map stationary (e.g. used internally for scroll zoom and double-click zoom).

        // setZoomAround(<Point> offset, <Number> zoom, <Zoom options> options)	this	
        // Zooms the map while keeping a specified pixel on the map (relative to the top-left corner) stationary.

        // fitBounds(<LatLngBounds> bounds, <fitBounds options> options?)	this	
        // Sets a map view that contains the given geographical bounds with the maximum zoom level possible.

        // fitWorld(<fitBounds options> options?)	this	
        // Sets a map view that mostly contains the whole world with the maximum zoom level possible.

        // panTo(<LatLng> latlng, <Pan options> options?)	this	
        // Pans the map to a given center.

        // panBy(<Point> offset, <Pan options> options?)	this	
        // Pans the map by a given number of pixels (animated).

        // flyTo(<LatLng> latlng, <Number> zoom?, <Zoom/pan options> options?)	this	
        // Sets the view of the map (geographical center and zoom) performing a smooth pan-zoom animation.

        // flyToBounds(<LatLngBounds> bounds, <fitBounds options> options?)	this	
        // Sets the view of the map with a smooth animation like flyTo, but takes a bounds parameter like fitBounds.

        // setMaxBounds(<LatLngBounds> bounds)	this	
        // Restricts the map view to the given bounds (see the maxBounds option).

        // setMinZoom(<Number> zoom)	this	
        // Sets the lower limit for the available zoom levels (see the minZoom option).

        // setMaxZoom(<Number> zoom)	this	
        // Sets the upper limit for the available zoom levels (see the maxZoom option).

        // panInsideBounds(<LatLngBounds> bounds, <Pan options> options?)	this	
        // Pans the map to the closest view that would lie inside the given bounds (if it's not already), controlling the animation using the options specific, if any.

        // panInside(<LatLng> latlng, <options> options?)	this	
        // Pans the map the minimum amount to make the latlng visible. Use padding, paddingTopLeft and paddingTopRight options to fit the display to more restricted bounds, like fitBounds. If latlng is already within the (optionally padded) display bounds, the map will not be panned.

        // invalidateSize(<Zoom/pan options> options)	this	
        // Checks if the map container size changed and updates the map if so — call it after you've changed the map size dynamically, also animating pan by default. If options.pan is false, panning will not occur. If options.debounceMoveend is true, it will delay moveend event so that it doesn't happen often even if the method is called many times in a row.

        // invalidateSize(<Boolean> animate)	this	
        // Checks if the map container size changed and updates the map if so — call it after you've changed the map size dynamically, also animating pan by default.

        // stop()	this	
        // Stops the currently running panTo or flyTo animation, if any.

        // Geolocation methods
        // Method	Returns	Description
        // locate(<Locate options> options?)	this	
        // Tries to locate the user using the Geolocation API, firing a locationfound event with location data on success or a locationerror event on failure, and optionally sets the map view to the user's location with respect to detection accuracy (or to the world view if geolocation failed). Note that, if your page doesn't use HTTPS, this method will fail in modern browsers (Chrome 50 and newer) See Locate options for more details.

        // stopLocate()	this	
        // Stops watching location previously initiated by map.locate({watch: true}) and aborts resetting the map view if map.locate was called with {setView: true}.

        // Other Methods
        // Method	Returns	Description
        // addHandler(<String> name, <Function> HandlerClass)	this	
        // Adds a new Handler to the map, given its name and constructor function.

        // remove()	this	
        // Destroys the map and clears all related event listeners.

        // createPane(<String> name, <HTMLElement> container?)	HTMLElement	
        // Creates a new map pane with the given name if it doesn't exist already, then returns it. The pane is created as a child of container, or as a child of the main map pane if not set.

        // getPane(<String|HTMLElement> pane)	HTMLElement	
        // Returns a map pane, given its name or its HTML element (its identity).

        // getPanes()	Object	
        // Returns a plain object containing the names of all panes as keys and the panes as values.

        // getContainer()	HTMLElement	
        // Returns the HTML element that contains the map.

        // whenReady(<Function> fn, <Object> context?)	this	
        // Runs the given function fn when the map gets initialized with a view (center and zoom) and at least one layer, or immediately if it's already initialized, optionally passing a function context.

        // Methods for Getting Map State
        // Method	Returns	Description
        // getCenter()	LatLng	
        // Returns the geographical center of the map view

        // getZoom()	Number	
        // Returns the current zoom level of the map view

        // getBounds()	LatLngBounds	
        // Returns the geographical bounds visible in the current map view

        // getMinZoom()	Number	
        // Returns the minimum zoom level of the map (if set in the minZoom option of the map or of any layers), or 0 by default.

        // getMaxZoom()	Number	
        // Returns the maximum zoom level of the map (if set in the maxZoom option of the map or of any layers).

        // getBoundsZoom(<LatLngBounds> bounds, <Boolean> inside?, <Point> padding?)	Number	
        // Returns the maximum zoom level on which the given bounds fit to the map view in its entirety. If inside (optional) is set to true, the method instead returns the minimum zoom level on which the map view fits into the given bounds in its entirety.

        // getSize()	Point	
        // Returns the current size of the map container (in pixels).

        // getPixelBounds()	Bounds	
        // Returns the bounds of the current map view in projected pixel coordinates (sometimes useful in layer and overlay implementations).

        // getPixelOrigin()	Point	
        // Returns the projected pixel coordinates of the top left point of the map layer (useful in custom layer and overlay implementations).

        // getPixelWorldBounds(<Number> zoom?)	Bounds	
        // Returns the world's bounds in pixel coordinates for zoom level zoom. If zoom is omitted, the map's current zoom level is used.

        // Conversion Methods
        // Method	Returns	Description
        // getZoomScale(<Number> toZoom, <Number> fromZoom)	Number	
        // Returns the scale factor to be applied to a map transition from zoom level fromZoom to toZoom. Used internally to help with zoom animations.

        // getScaleZoom(<Number> scale, <Number> fromZoom)	Number	
        // Returns the zoom level that the map would end up at, if it is at fromZoom level and everything is scaled by a factor of scale. Inverse of getZoomScale.

        // project(<LatLng> latlng, <Number> zoom)	Point	
        // Projects a geographical coordinate LatLng according to the projection of the map's CRS, then scales it according to zoom and the CRS's Transformation. The result is pixel coordinate relative to the CRS origin.

        // unproject(<Point> point, <Number> zoom)	LatLng	
        // Inverse of project.

        // layerPointToLatLng(<Point> point)	LatLng	
        // Given a pixel coordinate relative to the origin pixel, returns the corresponding geographical coordinate (for the current zoom level).

        // latLngToLayerPoint(<LatLng> latlng)	Point	
        // Given a geographical coordinate, returns the corresponding pixel coordinate relative to the origin pixel.

        // wrapLatLng(<LatLng> latlng)	LatLng	
        // Returns a LatLng where lat and lng has been wrapped according to the map's CRS's wrapLat and wrapLng properties, if they are outside the CRS's bounds. By default this means longitude is wrapped around the dateline so its value is between -180 and +180 degrees.

        // wrapLatLngBounds(<LatLngBounds> bounds)	LatLngBounds	
        // Returns a LatLngBounds with the same size as the given one, ensuring that its center is within the CRS's bounds. By default this means the center longitude is wrapped around the dateline so its value is between -180 and +180 degrees, and the majority of the bounds overlaps the CRS's bounds.

        // distance(<LatLng> latlng1, <LatLng> latlng2)	Number	
        // Returns the distance between two geographical coordinates according to the map's CRS. By default this measures distance in meters.

        // containerPointToLayerPoint(<Point> point)	Point	
        // Given a pixel coordinate relative to the map container, returns the corresponding pixel coordinate relative to the origin pixel.

        // layerPointToContainerPoint(<Point> point)	Point	
        // Given a pixel coordinate relative to the origin pixel, returns the corresponding pixel coordinate relative to the map container.

        // containerPointToLatLng(<Point> point)	LatLng	
        // Given a pixel coordinate relative to the map container, returns the corresponding geographical coordinate (for the current zoom level).

        // latLngToContainerPoint(<LatLng> latlng)	Point	
        // Given a geographical coordinate, returns the corresponding pixel coordinate relative to the map container.

        // mouseEventToContainerPoint(<MouseEvent> ev)	Point	
        // Given a MouseEvent object, returns the pixel coordinate relative to the map container where the event took place.

        // mouseEventToLayerPoint(<MouseEvent> ev)	Point	
        // Given a MouseEvent object, returns the pixel coordinate relative to the origin pixel where the event took place.

        // mouseEventToLatLng(<MouseEvent> ev)	LatLng	
        // Given a MouseEvent object, returns geographical coordinate where the event took place.

        // Properties
        // Controls
        // Property	Type	Description
        // zoomControl	Control.Zoom	The default zoom control (only available if the zoomControl option was true when creating the map).
        // Handlers
        // Property	Type	Description
        // boxZoom	Handler	Box (shift-drag with mouse) zoom handler.
        // doubleClickZoom	Handler	Double click zoom handler.
        // dragging	Handler	Map dragging handler (by both mouse and touch).
        // keyboard	Handler	Keyboard navigation handler.
        // scrollWheelZoom	Handler	Scroll wheel zoom handler.
        // tap	Handler	Mobile touch hacks (quick tap and touch hold) handler.
        // touchZoom	Handler	Touch zoom handler.
        // Map panes
        // Panes are DOM elements used to control the ordering of layers on the map. You can access panes with map.getPane or map.getPanes methods. New panes can be created with the map.createPane method.
        // Every map has the following default panes that differ only in zIndex.

        // Pane	Type	Z-index	Description
        // mapPane	HTMLElement	'auto'	Pane that contains all other map panes
        // tilePane	HTMLElement	200	Pane for GridLayers and TileLayers
        // overlayPane	HTMLElement	400	Pane for overlay shadows (e.g. Marker shadows)
        // shadowPane	HTMLElement	500	Pane for vectors (Paths, like Polylines and Polygons), ImageOverlays and VideoOverlays
        // markerPane	HTMLElement	600	Pane for Icons of Markers
        // tooltipPane	HTMLElement	650	Pane for Tooltips.
        // popupPane	HTMLElement	700	Pane for Popups.
        // Locate options
        // Some of the geolocation methods for Map take in an options parameter. This is a plain javascript object with the following optional components:
        // Option	Type	Default	Description
        // watch	Boolean	false	If true, starts continuous watching of location changes (instead of detecting it once) using W3C watchPosition method. You can later stop watching using map.stopLocate() method.
        // setView	Boolean	false	If true, automatically sets the map view to the user location with respect to detection accuracy, or to world view if geolocation failed.
        // maxZoom	Number	Infinity	The maximum zoom for automatic view setting when using setView option.
        // timeout	Number	10000	Number of milliseconds to wait for a response from geolocation before firing a locationerror event.
        // maximumAge	Number	0	Maximum age of detected location. If less than this amount of milliseconds passed since last geolocation response, locate will return a cached location.
        // enableHighAccuracy	Boolean	false	Enables high accuracy, see description in the W3C spec.
        // Zoom options
        // Some of the Map methods which modify the zoom level take in an options parameter. This is a plain javascript object with the following optional components:
        // Option	Type	Default	Description
        // animate	Boolean		If not specified, zoom animation will happen if the zoom origin is inside the current view. If true, the map will attempt animating zoom disregarding where zoom origin is. Setting false will make it always reset the view completely without animation.
        // Pan options
        // Some of the Map methods which modify the center of the map take in an options parameter. This is a plain javascript object with the following optional components:
        // Option	Type	Default	Description
        // animate	Boolean		If true, panning will always be animated if possible. If false, it will not animate panning, either resetting the map view if panning more than a screen away, or just setting a new offset for the map pane (except for panBy which always does the latter).
        // duration	Number	0.25	Duration of animated panning, in seconds.
        // easeLinearity	Number	0.25	The curvature factor of panning animation easing (third parameter of the Cubic Bezier curve). 1.0 means linear animation, and the smaller this number, the more bowed the curve.
        // noMoveStart	Boolean	false	If true, panning won't fire movestart event on start (used internally for panning inertia).
        // Zoom/pan options
        // ▶ Options inherited from Zoom options
        // ▶ Options inherited from Pan options
        // FitBounds options
        // Option	Type	Default	Description
        // paddingTopLeft	Point	[0, 0]	Sets the amount of padding in the top left corner of a map container that shouldn't be accounted for when setting the view to fit bounds. Useful if you have some control overlays on the map like a sidebar and you don't want them to obscure objects you're zooming to.
        // paddingBottomRight	Point	[0, 0]	The same for the bottom right corner of the map.
        // padding	Point	[0, 0]	Equivalent of setting both top left and bottom right padding to the same value.
        // maxZoom	Number	null	The maximum possible zoom to use.

      });
      L.tileLayer('http://t{s}.tianditu.gov.cn/img_w/wmts?tk=' + this.tk + '&SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TileMatrix={z}&TileCol={x}&TileRow={y}', {
        subdomains: [0, 1, 2, 3, 4, 5, 6, 7],
        opacity: 0.9
      }).addTo(this.map);

      // builds.addTo(this.map);
      this.initEvent();
      this.initUILayers();

      this.map.setView([24.529594499216834, 118.16049098968507], 17);

    },
    initEvent() {

      // Events
      // Layer events
      // Event	Data	Description
      // baselayerchange	LayersControlEvent	Fired when the base layer is changed through the layers control.
      // overlayadd	LayersControlEvent	Fired when an overlay is selected through the layers control.
      // overlayremove	LayersControlEvent	Fired when an overlay is deselected through the layers control.
      // layeradd	LayerEvent	Fired when a new layer is added to the map.
      // layerremove	LayerEvent	Fired when some layer is removed from the map

      // Map state change events
      // Event	Data	Description
      // zoomlevelschange	Event	Fired when the number of zoomlevels on the map is changed due to adding or removing a layer.
      // resize	ResizeEvent	Fired when the map is resized.
      // unload	Event	Fired when the map is destroyed with remove method.
      // viewreset	Event	Fired when the map needs to redraw its content (this usually happens on map zoom or load). Very useful for creating custom overlays.
      // load	Event	Fired when the map is initialized (when its center and zoom are set for the first time).
      // zoomstart	Event	Fired when the map zoom is about to change (e.g. before zoom animation).
      // movestart	Event	Fired when the view of the map starts changing (e.g. user starts dragging the map).
      // zoom	Event	Fired repeatedly during any change in zoom level, including zoom and fly animations.
      // move	Event	Fired repeatedly during any movement of the map, including pan and fly animations.
      // zoomend	Event	Fired when the map has changed, after any animations.
      // moveend	Event	Fired when the center of the map stops changing (e.g. user stopped dragging the map).

      // Popup events
      // Event	Data	Description
      // popupopen	PopupEvent	Fired when a popup is opened in the map
      // popupclose	PopupEvent	Fired when a popup in the map is closed
      // autopanstart	Event	Fired when the map starts autopanning when opening a popup.

      // Tooltip events
      // Event	Data	Description
      // tooltipopen	TooltipEvent	Fired when a tooltip is opened in the map.
      // tooltipclose	TooltipEvent	Fired when a tooltip in the map is closed.

      // Location events
      // Event	Data	Description
      // locationerror	ErrorEvent	Fired when geolocation (using the locate method) failed.
      // locationfound	LocationEvent	Fired when geolocation (using the locate method) went successfully.

      // Interaction events
      // Event	Data	Description
      // click	MouseEvent	Fired when the user clicks (or taps) the map.
      // dblclick	MouseEvent	Fired when the user double-clicks (or double-taps) the map.
      // mousedown	MouseEvent	Fired when the user pushes the mouse button on the map.
      // mouseup	MouseEvent	Fired when the user releases the mouse button on the map.
      // mouseover	MouseEvent	Fired when the mouse enters the map.
      // mouseout	MouseEvent	Fired when the mouse leaves the map.
      // mousemove	MouseEvent	Fired while the mouse moves over the map.
      // contextmenu	MouseEvent	Fired when the user pushes the right mouse button on the map, prevents default browser context menu from showing if there are listeners on this event. Also fired on mobile when the user holds a single touch for a second (also called long press).
      // keypress	KeyboardEvent	Fired when the user presses a key from the keyboard that produces a character value while the map is focused.
      // keydown	KeyboardEvent	Fired when the user presses a key from the keyboard while the map is focused. Unlike the keypress event, the keydown event is fired for keys that produce a character value and for keys that do not produce a character value.
      // keyup	KeyboardEvent	Fired when the user releases a key from the keyboard while the map is focused.
      // preclick	MouseEvent	Fired before mouse click on the map (sometimes useful when you want something to happen on click before any existing click handlers start running).

      // Other Events
      // Event	Data	Description
      // zoomanim	ZoomAnimEvent	Fired at least once per zoom animation. For continuous zoom, like pinch zooming, fired once per frame during zoom.

      this.map.on('click', e => {
        console.info("click", e);
      });
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
    initUILayers() {
      // marker 标注
      L.marker(this.center).addTo(this.map)
        .bindPopup('A pretty CSS3 popup.<br> Easily customizable.')
        .openPopup();
      // circle 圆形
      L.circle(this.center, {
        color: 'red',
        fillColor: '#f03',
        fillOpacity: 0.5,
        radius: 500
      }).addTo(this.map);
      // polygon 多边形
      L.polygon([
        [24.487713, 118.129859],
        [24.487713, 118.139859],
        [24.497713, 118.159859],
        [24.497713, 118.149859]
      ]).addTo(this.map);
      // Popup

      // Tooltip

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
</style>
