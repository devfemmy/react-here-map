
# react-here-map

React components for rendering and working with
[Here Maps](https://www.here.com/).

It simplifies the use of the Here Map JavaScript API with the help React
components.

The components can be imported and easily rendered. It also comes with seamless
configuration and modifications.

## Demo

[See it here](https://limistah.github.io/react-here-map/)

## Installation

Using NPM:

---
```bash
npm i --save react-here-map
```
---

Using Yarn:

---
```bash
yarn add react-here-map
```
---

## General Usage

---
```js
import React from "react";
import ReactDOM from "react-dom";
import HPlatform, { HMap, HMapPolyLine } from "react-here-map";

const points = [
  { lat: 52.5309825, lng: 13.3845921 },
  { lat: 52.5311923, lng: 13.3853495 },
  { lat: 52.5313532, lng: 13.3861756 },
  { lat: 52.5315142, lng: 13.3872163 },
  { lat: 52.5316215, lng: 13.3885574 },
  { lat: 52.5320399, lng: 13.3925807 },
  { lat: 52.5321472, lng: 13.3935785 },
];

ReactDOM.render(
  <HPlatform
    app_id="YOUR_APP_ID"
    apikey={"YOUR_API_KEY_FOR_V3.1"}
    useCIT
    useHTTPS
    includeUI
    includePlaces
  >
    <HMap
      style={{
        height: "400px",
        width: "800px",
      }}
      mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 }, zoom: 10 }}
    >
      <HMapPolyLine points={points} />
    </HMap>
  </HPlatform>,
  document.getElementById("app")
);
```
---

## Docs

The default export from this library instantiates the Here Maps platform, which
is required before initialization of other libraries of the Here Map Javascript
SDK.

The platform is expected to be a direct parent of all other supported
components.

Interesting components are also exported, they can be customized using supported
props.

### Components

1. **[HPlatform](#HPlatform)** - Platform initializer. All major components
   should be its direct child.
1. **[HMap](#hmap)** - Default export from this module, should be used as a
   parent for other components
1. **[HMapPolyLine](#hmappolyline)** - Draws a polyline on the map. A direct
   child of the map
1. **[HMapPolygon](#hmappolygon)** - Draws a polygon on the map. A direct child
   of the map
1. **[HMapMarker](#hmapmarker)** - Puts a marker on the map. A direct child of
   the map
1. **[HMapCircle](#hmapcircle)** - Draws a circle on the map. A direct child of
   the map
1. **[HMapRectangle](#hmaprectangle)** - Draws a rectangle on the map. A direct
   child of the map
1. **[Event Handling](#event-handling)** - Handling of user evens with `HMap` and it objects
   child of the map
1. **[HMapGeoCode](#hmapgeocode)** - Turns a physical address to a point on the
   map
1. **[HMapRoute](#hmaproute)** - Defines a route to locate two points. A direct
   child of the map
1. **[HMapLayer](#hmaplayer)** - Adds additional informational layer on the map.
   A direct child of the map

### Usage in details

#### HPlatform

A container for all of the components in this library. Generates a platform that
are injected into all of its direct children.

---
```js
import HPlatform, { HMap, HMapPolyLine } from "react-here-map";

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  {/*Children come in here*/}
</HPlatform>;
```
---

All direct children of `HPlatform` component receives:

- **platform** A reference to H.service.platform
  [Docs](https://developer.here.com/documentation/maps/topics_api/h-service-platform.html)
- **options** A reference to the options used to bootstrap the scripts.
  [See here](https://www.npmjs.com/package/here-map-js):

##### props

Props were determined by the options required for initializing a platform.

- **version** PropTypes.string - One of the supported version. Defaults to
  `'v3/3.0'`
- **app_id** PropTypes.string.isRequired - Application ID from account dashboard
- **app_code** PropTypes.string.isRequired - Application Code from account dashboard
- **app_code** PropTypes.string.isRequired - API KEY for usage with version 3.1Code from account
  dashboard
- **mapType** PropTypes.string - One of the above types accessed as a dot prop.
  Default `'normal.map'`
- **interactive** PropTypes.bool - Makes the map react to events. Needed for
  event handling
- **includeUI** PropTypes.bool - Add the UI controls
- **includePlaces** PropTypes.bool - Add the module for working with places
- **useCIT** PropTypes.boolean - Default to `true`
- **useHTTPS** PropTypes.boolean - Load the library using HTTPS. Default to
  `true`

#### HMap

Displays a Map for the types passed as props or default `normal.map`

Map types

---
```js
{
  normal: ["map", "traffic", "panorama", "transit", "base", "xbase", "labels"],
  satellite: ["xbase", "base", "map", "traffic", "panorama", "labels"],
  terrain: ["xbase", "base", "map", "traffic", "panorama", "labels"],
  incidents: true
}
```
---

All direct children of `HMap` component receives:

- **map** A reference to the map object used to create the visual map.
  [Docs](https://developer.here.com/documentation/maps/topics_api/h-map.html)
- **platform** A reference to H.service.platform
  [Docs](https://developer.here.com/documentation/maps/topics_api/h-service-platform.html)
- **ui** A reference to the ui object that does inclusion of ui elements.
  [Docs](https://developer.here.com/documentation/maps/topics_api/h-ui-intro.html)
- **\_\_options** A reference to the options merged with writable defaults used
  in bootsrapping the map and its items

_In any case you wish to render a supported component of this library outside
the context of the map, make sure to render in a place where the above props can
be passed explicitly to avoid nasty, unfriendly errors._

_In some cases as we will soon see, there is an option for passing a custom
component with more enhancements (defined by the programmer), these props are
received as first class directly from the containing parent and not from HMap,
but still holds same object's reference_

##### props

- **mapEvents** PropTypes.object -
  [officially supported events](https://developer.here.com/documentation/maps/topics/events.html)
- **mapOptions** PropTypes.object -
  [officially supported options](https://developer.here.com/documentation/maps/topics_api/h-map-options.html)

---
```js
import HPlatform, { HMap, HMapPolyLine } from "react-here-map";

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  />
</HPlatform>;
```
---

#### HMapPolyLine

Draws a polyline on the map

##### Props

- **points**: PropTypes.array.isRequired - Array of objects containing lat and
  lng
- **options**: PropTypes.object -
  [Officially supported options](https://developer.here.com/documentation/maps/topics_api/h-map-polyline-options.html)
- **setViewBounds**: PropTypes.bool - Makes the line centered in the container.
  Default `true`

#### Usage

---
```js
import HPlatform, { HMap, HMapPolyLine } from "react-here-map";
const points = [
  { lat: 52.5309825, lng: 13.3845921 },
  { lat: 52.5311923, lng: 13.3853495 },
  { lat: 52.5313532, lng: 13.3861756 },
  { lat: 52.5315142, lng: 13.3872163 },
  { lat: 52.5316215, lng: 13.3885574 },
  { lat: 52.5320399, lng: 13.3925807 },
  { lat: 52.5321472, lng: 13.3935785 },
];

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapPolyLine points={points} />
  </HMap>
</HPlatform>;
```
---

#### HMapPolygon

Draws a polygon on the map

##### Props

- **points**: PropTypes.array.isRequired - Array containing points or an array
  of lat,lng string separated by comma.
- **options**: PropTypes.object - options for the polygon.
  [Docs](https://developer.here.com/documentation/maps/topics_api/h-map-spatial-options.html#h-map-spatial-options)

#### Usage

---
```js
import HPlatform, { HMap, HMapPolygon } from "react-here-map";

const polygonPoints = [52, 13, 100, 48, 2, 100, 48, 16, 100, 52, 13, 100];
const polygonOptions = {
  style: {
    fillColor: "#FFFFCC",
    strokeColor: "#829",
    lineWidth: 8,
  },
};

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapPolygon
      points={polygonPoints}
      options={polygonOptions}
      setViewBounds="true"
    />
  </HMap>
</HPlatform>;
```
---

#### HMapMarker

Puts a marker on the map

#### Props

- **coords**: PropTypes.object.isRequired Object with lat and lng for the marker
- **icon**: PropTypes.any.isRequired Icon for the marker
- **options** PropTypes.object
  [Officially documented Options](https://developer.here.com/documentation/maps/topics_api/h-map-marker-options.html)
- **type**: PropTypes.string One of `undefined` | `DOM`. Default `undefined`
- **setViewBounds**: PropTypes.bool Centers the map with the marker. Default
  `true`

#

### Usage

---
```js
import HPlatform, { HMap, HMapMarker } from "react-here-map";

const coords = { lat: 52.5309825, lng: 13.3845921 };

const icon =
  '<svg width="24" height="24" ' +
  'xmlns="http://www.w3.org/2000/svg">' +
  '<rect stroke="white" fill="#1b468d" x="1" y="1" width="22" ' +
  'height="22" /><text x="12" y="18" font-size="12pt" ' +
  'font-family="Arial" font-weight="bold" text-anchor="middle" ' +
  'fill="white">H</text></svg>';

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapMarker coords={coords} icon={icon} />
  </HMap>
</HPlatform>;
```
---

#### HMapCircle

Puts a circle on the map

##### Props

- **coords**: PropTypes.object.isRequired - Object with lat and lng for the
  circle center point on the map
- **options**: PropTypes.object - Options for the circle.
  [Docs](https://developer.here.com/documentation/maps/topics_api/h-map-circle-options.html)
- **radius**: PropTypes.number - How large should one edge of the circle to the
  center point
- **setViewBounds**: PropTypes.bool - Centers the map with the circle. Default
  `true`

##### Usage

---
```js
import HPlatform, { HMap, HMapCircle } from "react-here-map";

const circleCoords = { lat: 52.5309825, lng: 13.3845921 };

const circleOptions = {
  style: {
    strokeColor: "rgba(55, 85, 170, 0.6)", // Color of the perimeter
    lineWidth: 2,
    fillColor: "rgba(0, 128, 0, 0.7)", // Color of the circle
  },
};
<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapCircle coords={circleCoords} radius={10000} options={circleOptions} />
  </HMap>
</HPlatform>;
```
---

#### HMapRectangle

Puts a rectangle on the map

##### Props

- **points**: PropTypes.array.isRequired - Four element array of point defining
  the boundaries of the rectangle
- **options**: PropTypes.object - Options for the rectangle.
  [Docs](https://developer.here.com/documentation/maps/topics_api/h-map-spatial-options.html#h-map-spatial-options)
- **setViewBounds**: PropTypes.bool - Centers the map with the circle. Default
  `true`

##### Usage

---
```js
import HPlatform, { HMap, HMapRectangle } from "react-here-map";

const points = [53.1, 13.1, 43.1, 40.1];
const rectangleOptions = {
  style: {
    fillColor: "#FFFFCC",
    strokeColor: "#E8FA75",
    lineWidth: 8,
  },
};
<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapRectangle points={points} options={rectangleOptions} />
  </HMap>
</HPlatform>;
```
---

### Event Handling

This section demonstrates how to use an event with either `HMap` component or a
map object.

#### HMap Event Handling

No additional prop is required aside from those requiredby `HMap`. Below is a
working code for a `pointerup` event:

To use an event, you have to pass interactive to the `HPlatform` and pass in
`useEvents` and `mapEvents` props to the `HMap` like this:

---
```js
import HPlatform, { HMap, HMapPolyLine } from "react-here-map";

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  interactive // Required for events
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
    useEvents // Required for events
    mapEvents={{ pointerdown: (e) => console.log("Map Pointer Down", e) }} // event handlers
  />
</HPlatform>;
```
---

#### Map Object event handling

All map object handles events the same way. Since all map objects are direct
children of `HMap` and receives `HPlatform` information, `useEvents` props in
the `HMap` tells the map objects to initialize events. So, a single `useEvents`
props is sufficient for all the children. In case that only a map object is
expected to handle events, `useEvents` can be passed to the object which will
initialize events defined for that object and not on the rest of the other
sibling objects of the same `HMap` parent.

##### Usage

---
```js
import HPlatform, {
  HMap,
  HMapCircle,
  HMapMarker,
  HMapPolygon,
  HMapPolyLine,
  HMapRectangle,
} from "react-here-map";

const rectanglePoints = [51.5072, 0, 48.8567, 2.3508];
const rectangleOptions = {
  style: {
    fillColor: "#FFFFCC",
    strokeColor: "#E8FA75",
    lineWidth: 8,
  },
};
const circleCoords = { lat: 52.3667, lng: 4.9 };
const circleOptions = {
  style: {
    strokeColor: "rgba(55, 85, 170, 0.6)", // Color of the perimeter
    lineWidth: 2,
    fillColor: "rgba(0, 128, 0, 0.7)", // Color of the circle
  },
};

const polyLinePoints = [
  { lat: 52.5167, lng: 13.3833 },
  { lat: 50.0833, lng: 14.4167 },
  { lat: 52.2333, lng: 21.0167 },
];

const polygonPoints = [45.4667, 9.1833, 0, 48.1333, 11.566, 0, 50.08, 8.24, 0];
const polygonOptions = {
  style: {
    fillColor: "#FFFFCC",
    strokeColor: "#829",
    lineWidth: 8,
  },
};

const markerCoords = { lat: 48.2, lng: 16.3667 };

const markerIcon =
  '<svg width="24" height="24" ' +
  'xmlns="http://www.w3.org/2000/svg">' +
  '<rect stroke="white" fill="#1b468d" x="1" y="1" width="22" ' +
  'height="22" /><text x="12" y="18" font-size="12pt" ' +
  'font-family="Arial" font-weight="bold" text-anchor="middle" ' +
  'fill="white">H</text></svg>';

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  interactive // Required for events
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    useEvents // Required for events
    mapEvents={{ pointerdown: (e) => console.log("Map Pointer Down", e) }} // event handlers
    mapOptions={{
      center: { lat: 51, lng: 7 },
      zoom: 5,
      pixelRatio: window.devicePixelRatio || 1,
    }}
  >
    <HMapCircle
      coords={circleCoords}
      radius={198000}
      options={circleOptions}
      objectEvents={{
        pointerdown: (e) => console.log("Circle Pointer Down", e),
      }}
    />
    <HMapRectangle
      points={rectanglePoints}
      options={rectangleOptions}
      objectEvents={{
        pointerdown: (e) => console.log("Rectangle Pointer Down", e),
      }}
    />
    <HMapPolyLine
      points={polyLinePoints}
      objectEvents={{
        pointerdown: (e) => console.log("Polyline Pointer Down", e),
      }}
    />
    <HMapPolygon
      points={polygonPoints}
      options={polygonOptions}
      objectEvents={{
        pointerdown: (e) => console.log("Polygon Pointer Down", e),
      }}
    />
    <HMapMarker
      coords={markerCoords}
      icon={markerIcon}
      objectEvents={{
        pointerdown: (e) => console.log("Marker Pointer Down", e),
      }}
    />
  </HMap>
</HPlatform>;
```
---

#### HMapGeoCode

> This uses React Hooks. Ensure that your react installation supports The Hooks
> API

##### Props

- **geoCodeParams**: PropTypes.object - Depends on the type being used.
  [Default params](https://developer.here.com/documentation/geocoder/topics/resource-geocode.html)
  to be used when reverse and landmark are falsy,
  [reverse params](https://developer.here.com/documentation/geocoder/topics/resource-reverse-geocode.html)
  to be used when reverse is set to true,
  [landmark params ](https://developer.here.com/documentation/geocoder/topics/resource-search.html)
  to be used when landmark is set to true
- **children**: PropTypes.element.isRequired - React Element that receives
  `map, platform, lat, lng` as props
- **reverse**: PropTypes.bool - Should implement reverse geo coding
- **landmark**: PropTypes.bool - Should implement landmark geo coding

##### Usage

##### Address to positions

Converts an address to a position on the map

---
```js
import HPlatform, { HMap, HMapGeoCode, HMapMarker } from "react-here-map";

const geoCodeParams = {
  searchText: "200 S Mathilda Ave, Sunnyvale, CA",
};

const icon =
  '<svg width="24" height="24" ' +
  'xmlns="http://www.w3.org/2000/svg">' +
  '<rect stroke="white" fill="#1b468d" x="1" y="1" width="22" ' +
  'height="22" /><text x="12" y="18" font-size="12pt" ' +
  'font-family="Arial" font-weight="bold" text-anchor="middle" ' +
  'fill="white">H</text></svg>';

// Can render any map element, make sure to pass map and platform as props to the children to avoid unwarranted behavior
const GeoMarker = ({ map, platform, ui, lat, lng, key }) => (
  <HMapMarker
    coords={{ lat, lng }}
    map={map}
    platform={platform}
    key={key}
    icon={icon}
  />
);
// Child of HMapGeoCode receives same params as above.
<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapGeoCode geoCodeParams={geoCodeParams}>
      <GeoMarker />
    </HMapGeoCode>
  </HMap>
</HPlatform>;
```
---

##### Position to address(es)

Converts a position to address(es) on the map

---
```js
import HPlatform, { HMap, HMapGeoCode } from "react-here-map";
// Create the parameters for the reverse geocoding request:
const reverseGeoCodingParameters = {
  prox: "52.5309,13.3847,150",
  mode: "retrieveAddresses",
  maxresults: 1,
};
// Can render any map element, make sure to pass map and platform as props to the children to avoid unwarranted behavior
const ReverseGeoMarker = ({ map, platform, ui, lat, lng, location, key }) => {
  ui.addBubble(
    new H.ui.InfoBubble(
      { lat, lng },
      { content: location.Location.Address.Label }
    )
  );
  return null;
};

// Child of HMapGeoCode receives same params as above.
<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapGeoCode geoCodeParams={reverseGeoCodingParameters} reverse>
      <ReverseGeoMarker />
    </HMapGeoCode>
  </HMap>
</HPlatform>;
```
---

###### Landmark Point

Locate landmark positions on the map

---
```js
import HPlatform, { HMap, HMapGeoCode } from "react-here-map";

const LandmarkGeoMarker = ({
  map,
  platform,
  ui,
  lat,
  lng,
  location,
  key,
  _location,
}) => {
  ui.addBubble(new H.ui.InfoBubble({ lat, lng }, { content: _location.Name }));
  return null;
};
// Create the parameters for the landmark search request:
const landmarkSearchParameters = {
  searchText: "TXL",
};

// Child of HMapGeoCode receives same params as above.
<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapGeoCode geoCodeParams={landmarkSearchParameters} landmark>
      <LandmarkGeoMarker />
    </HMapGeoCode>
  </HMap>
</HPlatform>;
```
---

#### HMapRoute

> This uses React Hooks. Ensure that your react installation supports Hooks API

##### Displaying route on the Map Using normal line

Shows path to between two points based on params

##### Props

- **routeParams**: PropTypes.object -
  [Officially documented route params](https://developer.here.com/documentation/routing/topics/resource-parameters-common.html)
- **lineOptions**: PropTypes.object -
  [Officially supported poly line options](https://developer.here.com/documentation/maps/topics_api/h-map-polyline-options.html)
- **icon**: PropTypes.any - Icon to be used for the marker
- **markerOptions**: PropTypes.object -
  [Officially supported marker Options](https://developer.here.com/documentation/maps/topics_api/h-map-marker-options.html)
- **children**: PropTypes.element - React element that receives
  `map, platform, ui, route, key, routeShape` as props
- **renderDefaultLine**: PropTypes.bool - Should use default renderer instead of
  a custom renderer as children
- **isoLine**: PropTypes.bool - Use IsoLine instead of a Polyline

##### Usages

###### Using the default renderer

---
```js
import HPlatform, {
  HMap,
  HMapRoute,
  HMapMarker,
  HMapPolyLine,
} from "react-here-map";

// Create the parameters for the routing request:
var routeParams = {
  // The routing mode:
  mode: "fastest;car",
  // The start point of the route:
  waypoint0: "geo!50.1120423728813,8.68340740740811",
  // The end point of the route:
  waypoint1: "geo!52.5309916298853,13.3846220493377",
  // To retrieve the shape of the route we choose the route
  // representation mode 'display'
  representation: "display",
};
const routeLineOptions = {
  style: { strokeColor: "blue", lineWidth: 10 },
  arrows: { fillColor: "white", frequency: 2, width: 0.8, length: 0.7 },
};

const icon =
  '<svg width="24" height="24" ' +
  'xmlns="http://www.w3.org/2000/svg">' +
  '<rect stroke="white" fill="#1b468d" x="1" y="1" width="22" ' +
  'height="22" /><text x="12" y="18" font-size="12pt" ' +
  'font-family="Arial" font-weight="bold" text-anchor="middle" ' +
  'fill="white">H</text></svg>';

// Handles manipulation of the path between the two points
const RouteMarker = ({ map, platform, ui, route, key, routeShape }) => {
  // Retrieve the mapped positions of the requested waypoints:
  const startPoint = route.waypoint[0].mappedPosition;
  const endPoint = route.waypoint[1].mappedPosition;

  // Create a marker for the start point:
  const startMarker = { lat: startPoint.latitude, lng: startPoint.longitude };
  // Create a marker for the end point:
  const endMarker = { lat: endPoint.latitude, lng: endPoint.longitude };

  return (
    <React.Fragment>
      <HMapPolyLine points={routeShape} map={map} setViewBounds />
      <HMapMarker
        coords={startMarker}
        map={map}
        platform={platform}
        icon={icon}
        setViewBounds
      />
      <HMapMarker
        coords={endMarker}
        map={map}
        platform={platform}
        icon={icon}
        setViewBounds={false}
      />
    </React.Fragment>
  );
};

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapRoute
      routeParams={routeParams}
      icon={icon}
      defaultDisplay
      lineOptions={routeLineOptions}
    />
  </HMap>
</HPlatform>;
```
---

###### Using a custom renderer

---
```js
import HPlatform, {
  HMap,
  HMapRoute,
  HMapMarker,
  HMapPolyLine,
} from "react-here-map";

// Create the parameters for the routing request:
var routeParams = {
  // The routing mode:
  mode: "fastest;car",
  // The start point of the route:
  waypoint0: "geo!50.1120423728813,8.68340740740811",
  // The end point of the route:
  waypoint1: "geo!52.5309916298853,13.3846220493377",
  // To retrieve the shape of the route we choose the route
  // representation mode 'display'
  representation: "display",
};
const routeLineOptions = {
  style: { strokeColor: "blue", lineWidth: 10 },
  arrows: { fillColor: "white", frequency: 2, width: 0.8, length: 0.7 },
};

const icon =
  '<svg width="24" height="24" ' +
  'xmlns="http://www.w3.org/2000/svg">' +
  '<rect stroke="white" fill="#1b468d" x="1" y="1" width="22" ' +
  'height="22" /><text x="12" y="18" font-size="12pt" ' +
  'font-family="Arial" font-weight="bold" text-anchor="middle" ' +
  'fill="white">H</text></svg>';

// Handles manipulation of the path between the two points
const RouteMarker = ({ map, platform, ui, route, key, routeShape }) => {
  // Retrieve the mapped positions of the requested waypoints:
  const startPoint = route.waypoint[0].mappedPosition;
  const endPoint = route.waypoint[1].mappedPosition;

  // Create a marker for the start point:
  const startMarker = { lat: startPoint.latitude, lng: startPoint.longitude };
  // Create a marker for the end point:
  const endMarker = { lat: endPoint.latitude, lng: endPoint.longitude };

  return (
    <React.Fragment>
      <HMapPolyLine points={routeShape} map={map} setViewBounds />
      <HMapMarker
        coords={startMarker}
        map={map}
        platform={platform}
        icon={icon}
        setViewBounds
      />
      <HMapMarker
        coords={endMarker}
        map={map}
        platform={platform}
        icon={icon}
        setViewBounds
      />
    </React.Fragment>
  );
};

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapRoute
      routeParams={routeParams}
      icon={icon}
      defaultDisplay
      lineOptions={routeLineOptions}
    >
      <RouteMarker />
    </HMapRoute>
  </HMap>
</HPlatform>;
```
---

#### Displaying route on the Map Using iso line

###### Using the default renderer

---
```js
import HPlatform, { HMap, HMapPolygon, HMapRoute } from "react-here-map";
// Create the parameters for the reverse geocoding request:
const isoRoutingParams = {
  mode: "fastest;car;",
  start: "geo!52.5,13.4",
  range: "900",
  rangetype: "time",
};

const routeLineOptions = {
  style: { strokeColor: "blue", lineWidth: 10 },
  arrows: { fillColor: "white", frequency: 2, width: 0.8, length: 0.7 },
};
const icon =
  '<svg width="24" height="24" ' +
  'xmlns="http://www.w3.org/2000/svg">' +
  '<rect stroke="white" fill="#1b468d" x="1" y="1" width="22" ' +
  'height="22" /><text x="12" y="18" font-size="12pt" ' +
  'font-family="Arial" font-weight="bold" text-anchor="middle" ' +
  'fill="white">H</text></svg>';

const RouteMarkerIso = ({
  map,
  platform,
  ui,
  route,
  routeShape,
  center,
  component,
}) => {
  return (
    <React.Fragment>
      <Polygon
        points={routeShape}
        options={polygonOptions}
        setViewBounds
        map={map}
        platform={platform}
      />
      <Marker
        coords={center}
        map={map}
        platform={platform}
        icon={icon}
        options={markerOptions}
        setViewBounds={false}
      />
    </React.Fragment>
  );
};

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
    interactive
    includeUI
  >
    <HMapRoute
      routeParams={isoRoutingParams}
      icon={icon}
      isoLine
      defaultDisplay
      lineOptions={routeLineOptions}
    />
  </HMap>
</HPlatform>;
```
---

###### Using a custom renderer

---
```js
import HPlatform, { HMap, HMapPolygon, HMapRoute } from "react-here-map";
// Create the parameters for the reverse geocoding request:
const isoRoutingParams = {
  mode: "fastest;car;",
  start: "geo!52.5,13.4",
  range: "900",
  rangetype: "time",
};

const routeLineOptions = {
  style: { strokeColor: "blue", lineWidth: 10 },
  arrows: { fillColor: "white", frequency: 2, width: 0.8, length: 0.7 },
};
const icon =
  '<svg width="24" height="24" ' +
  'xmlns="http://www.w3.org/2000/svg">' +
  '<rect stroke="white" fill="#1b468d" x="1" y="1" width="22" ' +
  'height="22" /><text x="12" y="18" font-size="12pt" ' +
  'font-family="Arial" font-weight="bold" text-anchor="middle" ' +
  'fill="white">H</text></svg>';
const RouteMarkerIso = ({
  map,
  platform,
  ui,
  route,
  routeShape,
  center,
  component,
}) => {
  return (
    <React.Fragment>
      <Polygon
        points={routeShape}
        options={polygonOptions}
        setViewBounds
        map={map}
        platform={platform}
      />
      <Marker
        coords={center}
        map={map}
        platform={platform}
        icon={icon}
        options={markerOptions}
        setViewBounds={false}
      />
    </React.Fragment>
  );
};

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
    interactive
    includeUI
  >
    <HMapRoute
      routeParams={isoRoutingParams}
      icon={icon}
      defaultDisplay={false}
      isoLine
      lineOptions={routeLineOptions}
    >
      <RouteMarkerIso />
    </HMapRoute>
  </HMap>
</HPlatform>;
```
---

#### HMapLayer

Adds a layer to the map.

Individual layer holds different information

##### props

**mapLayerType**: PropTypes.string.isRequired In a dot prop form e.g
`mapLayerType="incidents", mapLayerType="normal.traffic"`

---
```js
{
  normal: [
    "xbase",
    "xbasenight",
    "base",
    "basenight",
    "map",
    "mapnight",
    "traffic",
    "trafficnight",
    "transit",
    "panorama",
    "panoramanight",
    "labels",
    "metaInfo"
  ],
  satellite: ["xbase", "base", "map", "traffic", "panorama", "labels"],
  terrain: ["xbase", "base", "map", "traffic", "panorama", "labels"],
  incidents: true
}
```
---

##### Usage

---
```js
import HPlatform, { HMap, HMapLayer } from "react-here-map";

// Child of HMapGeoCode receives same params as above.
<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includeUI
  includePlaces
>
  <HMap
    style={{
      height: "400px",
      width: "800px",
    }}
    mapOptions={{ center: { lat: 52.5321472, lng: 13.3935785 } }}
  >
    <HMapLayer mapLayerType="normal.trafficnight" />
  </HMap>
</HPlatform>;
```
---

#### HMapPlaces

Search for places on the map

### Props

- **library**: PropTypes.string.isRequired One of the places library supported
  by here maps for the requests
- **query**: PropTypes.string Passing the query externally to initiate the
  request on load after getting the location of the user

- **category**: PropTypes.string Result category

- **placeClassName**: PropTypes.string Class for the container

- **inputClassName**: PropTypes.string Class for the Input field

- **containerStyle**: PropTypes.object Styles for the container
- **inputStyle**: PropTypes.object Styles for the input

- **itemContainerClass**: PropTypes.string Result Items container class

* **itemClass**: PropTypes.string Result Items class

* **iconClass**: PropTypes.string Icon marker class name

* **inputStyle**: PropTypes.object Styles for the input

* **getItem**: PropTypes.function Callback when an item is clicked in the result

- **markerOptions**: PropTypes.object Options for the marker

- **markerIcon**: PropTypes.element Icon for the marker

- **markerType**: PropTypes.string Type of marker icon

* **multiMarker**: PropTypes.boolean allow for many markers

### Usage

---
```js
import HPlatform, { HMap, HMapPlaces } from "react-here-map";

<HPlatform
  app_id="YOUR_APP_ID"
  apikey={"YOUR_API_KEY_FOR_V3.1"}
  useCIT
  useHTTPS
  includePlaces
  interactive
>
  <HMapPlaces library="search" />
</HPlatform>;
```
---

## CHANGES

**06/05/2020**

- Includes support for V3.1 API_KEY

## Contributions

See the TODO.MD

## Licence

MIT
