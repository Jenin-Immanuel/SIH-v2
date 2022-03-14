<script>
  import { onMount, onDestroy } from "svelte";
  import mapbox from "mapbox-gl";
  import "mapbox-gl/dist/mapbox-gl.css";

  const {
    Map,
    Marker,
    Popup,
    NavigationControl,
    FullscreenControl,
    GeolocateControl,
  } = mapbox;

  let map;
  mapbox.accessToken =
    "pk.eyJ1Ijoid2VyZGQxMjMiLCJhIjoiY2wwbWFhdWg4MTQ4OTNkbWdvMWJtM2FwdSJ9.OzscmVsBnX7zlZVzc-AhkQ";

  const EVS = [
    {
      lat: 11.10618,
      lng: 77.35042,
      name: "Sri Sakthi Cinemas",
      provider: "Zeon Chargers",
      ports: 1,
      chargingType: "Fast",
    },
    {
      lat: 11.175093,
      lng: 77.263374,
      name: "SREE ANNAPOORNA HOTEL",
      provider: "Zeon Chargers",
      ports: 1,
      chargingType: "Fast",
    },
    {
      lat: 11.213022,
      lng: 77.441968,
      name: "IOCL COCO",
      provider: "Tata Power - CCS2 & slow chargers",
      ports: 1,
      chargingType: "Normal",
    },
    {
      lat: 11.30199446,
      lng: 77.60004376,
      name: "RMS Towers (Ero_G Electric Charging Station)",
      provider: "Relux Electric",
      ports: 1,
      chargingType: "Fast",
    },
    {
      lat: 11.519342,
      lng: 77.935042,
      name: "SHREE SARAVANA BHAVAN",
      provider: "Zeon Chargers",
      ports: 4,
      chargingType: "Fast",
    },
    {
      lat: 11.5213,
      lng: 77.93434,
      name: "Sree Saravana Bhavan",
      provider: "Zeon Chargers",
      ports: 1,
      chargingType: "Fast",
    },
    {
      lat: 11.4922712,
      lng: 78.1535774,
      name: "Adyar Ananda Bhavan",
      provider: "Zeon Chargers",
      ports: 3,
      chargingType: "Fast",
    },
    {
      lat: 11.682985,
      lng: 78.1260402,
      name: "True Sai Motors",
      provider: "Tata Power - CCS2 & slow chargers",
      ports: 2,
      chargingType: "Normal",
    },
    {
      lat: 10.9984009,
      lng: 76.9765316,
      name: "Richy Rich",
      provider: "Ather",
      ports: 4,
      portTypes: { AC: 2, Ather: 2 },
      chargingType: "Fast",
    },
    {
      lat: 11.00269216,
      lng: 77.0387079,
      name: "IOCL Shanthi Social Services",
      provider: "Tata Power - CCS2 & slow chargers",
    },
    {
      lat: 11.04991,
      lng: 77.05765,
      name: "PPS Motors",
      provider: "Tata Power - CCS2 & slow chargers",
      ports: 2,
      chargingType: "Fast",
    },
  ];

  let currentLocation = {
    lat: 11.0,
    lng: 77.0,
  };
  let destination = {
    lat: 11.6643,
    lng: 78.146,
  };

  let routes = [];

  const getDirectionURL = (from, to) => {
    return `https://api.mapbox.com/directions/v5/mapbox/driving/${from.lng},${from.lat};${to.lng},${to.lat}?geometries=geojson&access_token=${mapbox.accessToken}`;
  };

  const getCurrentLocation = () => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          currentLocation = {
            lat: position.coords.latitude,
            lng: position.coords.longitude,
          };
        },
        (error) => {
          console.log(error);
        }
      );
    } else {
      console.log("Geolocation is not supported by this browser.");
    }
  };
  getCurrentLocation();

  onMount((_) => {
    map = new Map({
      container: "map",
      style: "mapbox://styles/mapbox/streets-v8",
      center: [77.0, 11.0],
      zoom: 10,
    });

    map.on("load", () => {
      map.addControl(new NavigationControl());
      map.addControl(new FullscreenControl());
      map.addControl(new GeolocateControl());
      EVS.forEach((ev) => {
        const { lng, lat, name } = ev;
        new Marker({
          color: "#ff0000",
        })
          .setLngLat([lng, lat])
          .setPopup(new Popup({ offset: 25 }).setHTML(`<h3>${name}</h3>`))
          .addTo(map);
      });
      fetch(getDirectionURL(currentLocation, destination))
        .then((res) => res.json())
        .then((data) => {
          routes = data.routes;
          console.log(routes);
          routes.forEach((route, i) => {
            map.addSource(`route-${i}`, {
              type: "geojson",
              data: route.geometry,
            });
            map.addLayer({
              id: `route-${i}`,
              type: "line",
              source: `route-${i}`,
              layout: {
                "line-join": "round",
                "line-cap": "round",
              },
              paint: {
                "line-color": i == 0 ? "#3887be" : "#3bb2d0",
                "line-width": 8,
              },
            });
          });
        });
    });
  });

  onDestroy((_) => map.remove());
</script>

<div>
  <div class="map" id="map" />
</div>

<style>
  .map {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
  }
</style>
