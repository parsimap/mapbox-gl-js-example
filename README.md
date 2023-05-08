# mapbox-gl-js-demo

This project is an example for implement a map in the JavaScript language without any facility or framework.

## SOURCES Variable

A dictionary to better access to source ids.

```javascript
const SOURCES = {
  region6_area: "region6_area",
  region6_important_streets: "region6_important_streets",
  region6_restaurant_points: "region6_restaurant_points",
};
```

## runSourceQueries

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

## Main function callable

The main function is called after page fully loaded and ready to use.
All featureCollection files are fetched and ready
to use as source-data for a map, and as a result is shown as a features on the map.

```javascript
(() => main())();
```

## Map Creation

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

## `style.load` Event

Secondly, The `style.load` event of the map is used when there isa need to add following sources or layers.

```javascript
map.on("style.load", () => {
});
```

## Adding Sources

Thirdly, All sources were fetched is ready to use as a source.This data is an arrayList on the other hand, the tuple
which the first member of each array record there are the id and the other are data.
```javascript
 for (const [id, data] of sourceArrayList) {
  map.addSource(id, { type: "geojson", data });
}
```