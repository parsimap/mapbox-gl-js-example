# mapbox-gl-js-demo

This project is an example for implement a map in the JavaScript language without any facility or framework.

## Including Files

The style and script of the `mapbox-gl-js` is added inside `header` tag of the html page.

```html

<script src="https://cdn.parsimap.ir/third-party/mapbox-gl-js/v1.13.0/mapbox-gl.js"></script>
<link
  href="https://cdn.parsimap.ir/third-party/mapbox-gl-js/v1.13.0/mapbox-gl.css"
  rel="stylesheet"
/>
```

## JavaScript

All the codes at here can be placed inside `body` tag.

### SOURCES Variable

A dictionary to better access to source ids.

```javascript
const SOURCES = {
  region6_area: "region6_area",
  region6_important_streets: "region6_important_streets",
  region6_restaurant_points: "region6_restaurant_points"
};
```

### runSourceQueries

This method as a chaining request run queries in sequence, all data is available in the data folder of this project.

```javascript
async function runSourceQueries() {
  return await Promise.all(
    Object.keys(SOURCES).map(async (id) => [
      id,
      await (await fetch(`data/${id}.json`)).json(),
    ])
  );
}
```

### Main function callable

The main function is called after page fully loaded and ready to use.
All featureCollection files are fetched and ready
to use as source-data for a map, and as a result is shown as a features on the map.

```javascript
(() => main())();
```

### Map Plugins

To illustration the text in the map for rtl languages, it is essential to set rtl plugin,after that the text in the map
in correct order will be presented.
```javascript
mapboxgl.setRTLTextPlugin(
  "https://cdn.parsimap.ir/third-party/mapbox-gl-js/plugins/mapbox-gl-rtl-text/v0.2.3/mapbox-gl-rtl-text.js",
  null
);
```

### Map Creation

Firstly, the map should be created from mapboxgl which is available globally after including the related script; here
the script is used from parsimap cdn.To properly show the map; there is a need for an access token which
the [following link](https://account.parsimap.ir/token-registration) could help you reach a Parsiamp company.

```javascript
const map = new mapboxgl.Map({
  container: "map",
  style:
    "https://api.parsimap.ir/styles/parsimap-streets-v11?key={PMI_TOKEN}",
  center: [51.4, 35.7],
  zoom: 8
});
```

### `style.load` Event

Secondly, The `style.load` event of the map is used when there isa need to add following sources or layers.

```javascript
map.on("style.load", () => {
});
```

### Adding Sources

Thirdly, All sources were fetched is ready to use as a source.This data is an arrayList on the other hand, the tuple
which the first member of each array record there are the id and the other are data.

```javascript
 for (const [id, data] of sourceArrayList) {
  map.addSource(id, { type: "geojson", data });
}
```

### Adding Layers

Fourthly, layers can be defined and this definition must be attached to related sources which were fetched before.

#### Area Layer

This layer is used to demonstrate an area which is related to a district of Tehran city.
The area is shown as a polygon and filled with color.

#### Street Layer

This layer is illustrated the stroke on the map for three well-known roads of the city.

#### Restaurant Layer

This layer is a circle layer that is displayed in each restaurant place as a circle on the map.