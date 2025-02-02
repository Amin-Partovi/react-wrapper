# Google Maps JavaScript API React Wrapper

[![npm](https://img.shields.io/npm/v/@googlemaps/react-wrapper)](https://www.npmjs.com/package/@googlemaps/react-wrapper)
![Build](https://github.com/googlemaps/react-wrapper/workflows/Test/badge.svg)
![Release](https://github.com/googlemaps/react-wrapper/workflows/Release/badge.svg)
[![codecov](https://codecov.io/gh/googlemaps/react-wrapper/branch/master/graph/badge.svg)](https://codecov.io/gh/googlemaps/react-wrapper)
![GitHub contributors](https://img.shields.io/github/contributors/googlemaps/react-wrapper?color=green)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)
[![](https://github.com/jpoehnelt/in-solidarity-bot/raw/main/static//badge-flat-square.png)](https://github.com/apps/in-solidarity)

## Description

Wrap React components with this library to load the Google Maps JavaScript API.

```jsx
import { Wrapper } from "@googlemaps/react-wrapper";

const MyApp = () => (
  <Wrapper apiKey={"YOUR_API_KEY"}>
    <MyMapComponent />
  </Wrapper>
);
```

The preceding example will not render any elements unless the Google Maps JavaScript API is successfully loaded. To handle error cases and the time until load is complete, it is recommended to provide render props.

```jsx
import { Wrapper, Status } from "@googlemaps/react-wrapper";

const render = (status) => {
  switch (status) {
    case Status.LOADING:
      return <Spinner />;
    case Status.FAILURE:
      return <ErrorComponent />;
    case Status.SUCCESS:
      return <MyMapComponent />;
  }
};

const MyApp = () => <Wrapper apiKey={"YOUR_API_KEY"} render={render} />;
```

When combining children and render props, the children will render on success and the render prop will be executed for other status values.

```tsx
import { Wrapper, Status } from "@googlemaps/react-wrapper";

const render = (status: Status): ReactElement => {
  if (status === Status.FAILURE) return <ErrorComponent />;
  return <Spinner />;
};

const MyApp = () => (
  <Wrapper apiKey={"YOUR_API_KEY"} render={render}>
    <MyMapComponent />
  </Wrapper>
);
```

### @googlemaps/js-api-loader

This wrapper uses [@googlemaps/js-api-loader][js_api_loader] to load the Google Maps JavaScript API. This library uses a singleton pattern and will not attempt to load the library more than once. All options accepted by [@googlemaps/js-api-loader][js_api_loader] are also accepted as props to the wrapper component.
### MyMapComponent

The following snippets demonstrates the usage of `useRef` and `useEffect` hooks with Google Maps.

```tsx
function MyMapComponent({
  center,
  zoom,
}: {
  center: google.maps.LatLngLiteral;
  zoom: number;
}) {
  const ref = useRef();

  useEffect(() => {
    new window.google.maps.Map(ref.current, {
      center,
      zoom,
    });
  });

  return <div ref={ref} id="map" />;
}
```

### Customizing Map Style and Marker Icon

To customize the map style in your project, you can utilize the googleMapStyle array in the styles configuration of your map. Additionally, you have the option to use a custom icon for the map marker. Here's an example code snippet to help you get started:

```tsx
 useEffect(() => {
    const map = new window.google.maps.Map(ref.current as HTMLElement, {
      ...mapOptions,
      center,
      styles: googleMapStyle,
    });

    new google.maps.Marker({
      position: center,
      map: map,
      icon: MapMarker(),
    });
  });
```



You can find a wide variety of map styles from resources like [Styling Wizard](https://mapstyle.withgoogle.com/) and [Snazzy Maps](https://snazzymaps.com/). These platforms offer an extensive collection of pre-defined styles for you to choose from.

To customize the marker icon, you can use the provided MapMarker function, which returns an object representing the icon configuration.

```tsx
const MapMarker = () => {
  if (typeof window === "undefined") return;
  return {
    path: "m12.84 20.142-...",
    fillColor: "#FDBA56",
    fillOpacity: 1,
    strokeWeight: 0,
    rotation: 0,
    scale: 1,
    anchor: new window.google.maps.Point(0, 20),
  };
};

export default MapMarker;
```

Don't forget to include the MapMarker configuration in the icon property of the Google Maps marker.

The map controls allow users to interact with the map and access various features. You can customize the presence of controls on the map by setting boolean values for the following props:

`zoomControl`: Enables the zoom control on the map.  
`mapTypeControl`: Enables the map type control for switching between different map types.  
`scaleControl`: Displays a scale bar on the map.  
`streetViewControl`: Enables the street view control for navigating street-level imagery.  
`panControl`: Enables the pan control for panning the map.  
`rotateControl`: Enables the rotate control for rotating the map.  
`fullscreenControl`: Enables the fullscreen control for expanding the map to fullscreen mode.  

To create custom options, you can define an options object and add the desired controls as shown in the example below:  

```jsx
const mapOptions = {
  zoomControl: true,
  mapTypeControl: false,
  scaleControl: true,
  streetViewControl: false,
  panControl: true,
  rotateControl: false,
  fullscreenControl: true,
};
```
```tsx
const map = new window.google.maps.Map(ref.current as HTMLElement, {
  ...mapOptions,
  center,
  zoom: 10,
});
```

By customizing these options, you can tailor the map controls to meet your specific needs.


## Examples

See the [examples](https://github.com/googlemaps/react-wrapper/tree/main/examples) folder for additional usage patterns.

* [Basic Demo](https://googlemaps.github.io/react-wrapper/public/basic/)
## Install

Available via npm as the package [@googlemaps/react-wrapper](https://www.npmjs.com/package/@googlemaps/react-wrapper).

```sh
npm i @googlemaps/react-wrapper
```

or

```sh
yarn add @googlemaps/react-wrapper
```

For TypeScript support additionally install type definitions.

```sh
npm i -D @types/google.maps
```

or

```sh
yarn add -D @types/google.maps
```

## Documentation

The reference documentation can be found at this [link](https://googlemaps.github.io/react-wrapper/index.html).


## Support

This library is community supported. We're comfortable enough with the stability and features of
the library that we want you to build real production applications on it.

If you find a bug, or have a feature suggestion, please [log an issue][issues]. If you'd like to
contribute, please read [How to Contribute][contrib].

[issues]: https://github.com/googlemaps/react-wrapper/issues
[contrib]: https://github.com/googlemaps/react-wrapper/blob/master/CONTRIBUTING.md
[js_api_loader]: https://www.npmjs.com/package/@googlemaps/js-api-loader
