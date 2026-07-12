# TravelTip
#### The app that gets you somewhere


## Description
TravelTip is an app that keeps a list of favorite locations on a Google Map.

## Main Features
- The app allows the user to keep and manage locations
- The user can also search for an address and pan the map to that point
- The user can pan the map to his own geo-location
- When the user's position is known, the distance to each location is shown

## Locations CRUDL
- **Create** – click on the map opens a dialog for name and rate
- **Read** – selected location details (see below)
- **Update** – edit a location's name and rate via the same dialog
- **Delete** – delete a location (with a confirmation prompt)
- **List** – including filtering, sorting and grouping

### Filtering
- By text – tested against both the location `name` and its `geo.address`
- By minimum rate

### Sorting
- By name
- By rate
- By creation date
- Each sort can be ascending or descending

### Grouping (pie charts)
- **By rate** – low / medium / high
- **By last update** – today / past / never

## Selected Location
- Displayed in the header, including the distance from the user (when known)
- Location is active in the list (gold color)
- Marker on the map
- Reflected in query params (bookmarkable, shareable url)
- Copy url to clipboard
- Share via Web-Share API

## Location
Here is the format of the location object:
```js
{
    id: 'GEouN',
    name: 'Dahab, Egypt',
    rate: 5,
    geo: {
      address: 'Dahab, South Sinai, Egypt',
      lat: 28.5096676,
      lng: 34.5165187,
      zoom: 11
    },
    createdAt: 1706562160181,
    updatedAt: 1706562160181
  }
  ```

## Services
```js
export const locService = {
    query,
    getById,
    remove,
    save,
    setFilterBy,
    setSortBy,
    getLocCountByRateMap,
    getLocCountByDateMap
}

export const mapService = {
    initMap,
    getUserPosition,
    setMarker,
    panTo,
    lookupAddressGeo,
    addClickListener
}

export const utilService = {
    saveToStorage,
    loadFromStorage,
    makeId,
    getRandomIntInclusive,
    randomPastTime,
    getRandomLatLng,
    elapsedTime,
    getColors,
    updateQueryParams,
    getDistance
}
```

## Controller
```js
// To make things easier in this project structure
// functions that are called from DOM are defined on a global app object

window.app = {
    onRemoveLoc,
    onUpdateLoc,
    onSelectLoc,
    onPanToUserPos,
    onSearchAddress,
    onCopyLoc,
    onShareLoc,
    onSetSortBy,
    onSetFilterBy,
    onShowModal,
    onSubmitModal
}
```

Here is a sample usage:
```html
<button onclick="app.onCopyLoc()">Copy location</button>
<button onclick="app.onShareLoc()">Share location</button>
```

## Add / Update Dialog
Adding and updating a location both use a single `<dialog>` modal (`.add-update-modal`)
instead of native `prompt()` calls. The location being edited is kept in the controller
variable `gTempLoc` while the dialog is open, so on submit its name and rate are applied
back to the correct location.

## Project Structure
```
css/
  base/      (vars.css, base.css)
  cmps/      (locs.css, selected-loc.css, user-msg.css, add-update-modal.css)
  main.css
img/
js/
  services/  (async-storage.service.js, loc.service.js, map.service.js, util.service.js)
  app.controller.js
index.html
```

## Setup
The map uses the Google Maps JavaScript API. Add your own API key in
`js/services/map.service.js` (the `API_KEY` constant) and make sure the
**Maps JavaScript API** and **Geocoding API** are enabled for it.
