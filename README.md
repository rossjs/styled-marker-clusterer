StyledMarkerClusterer for Google Maps v3
==================================

## Description

Forked version of the [Google Maps API v3 marker-clusterer utility library](https://github.com/googlemaps/v3-utility-library/tree/master/markerclusterer) that includes the ability to pass a callback and add dynamic custom styles to your marker clusters.

## NPM

Available via NPM as the package `rossjs/styled-marker-clusterer`

[Read more][more]

## Usage

Usage is exactly the same as Google's MarkerClusterer except you can now pass in a `styleFunc` property to the options object that must be a callback function.

The callback function will passed the markers array from each cluster instance. You can use any data that you've set on your markers to create dynamic styles. The `styleFunc` callback function should return an object that has a `styles` property which should be an object of styles with properties in a hyphenated format, just like normal CSS (bracket notation y'all!). Additionally the callback function can contain a `size` property that will affect the size of the default styling (filled circles).

```js
const data = {
  notSpecial: [
    { lat: 41.91658511622835, lng: -87.64961242675781 },
    { lat: 41.87723019276536, lng: -87.71003723144531 },
    { lat: 41.84450115262933, lng: -87.64961242675781 },
    { lat: 41.85728792769134, lng: -87.71553039550781 },
    { lat: 42.01308073078780, lng: -87.69081115722656 },
    { lat: 41.94825586972943, lng: -87.74093627929688 },
    { lat: 41.95897949510114, lng: -87.67364501953124 },
    { lat: 41.89205502378826, lng: -87.63038635253906 },
    { lat: 41.82147851660402, lng: -87.61253356933594 },
  ],
  special: [
    { lat: 41.83120019521289, lng: -87.67639160156251 },
    { lat: 41.93037915150084, lng: -87.76496887207031 },
    { lat: 41.79691191119474, lng: -87.65373229980469 },
    { lat: 41.80254259036435, lng: -87.60360717773436 },
  ],
};

// styling function to dynamically style clusters
const styleFunc = (markers) => {
  let size = 15;
  const styles = {};
  const isSpecial = markers.some(marker => marker.special);
  if (isSpecial) {
    size = 30;
    styles['background-color'] = 'purple';
  }
  return { size, styles };
}

// instantiate your map (after Google Maps API has loaded)
var map = new google.maps.Map(document.getElementById('map'), {
  zoom: 10,
  center: { lat: 41.9145, lng: -87.6627 }
});

const markers = [];

// add regular ole' markers, nothing to see here
data.notSpecial.forEach(point => {
  const marker = new google.maps.Marker({ position: point });
  markers.push(marker);
});

// add super special markers
data.special.forEach(point => {
  const marker = new google.maps.Marker({ position: point, special: true });
  markers.push(marker);
});

const cluster = new MarkerCluster(map, markers, { styleFunc });

```