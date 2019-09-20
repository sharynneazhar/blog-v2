---
title: "Hacking the Google Places Autocomplete API"
date: 2017-08-01T12:00:00-00:00
draft: false
tags:
  - react
  - google-api
---

For the past couple of weeks, I've been working on implementing a location search box using [Google Places API][1]. It would be used to apply a location filter on a list of user profiles. To do so, I needed to use the [Autocomplete API][2] which provides a nice type-ahead-search behavior you've seen in many of Google's search boxes. Once I got my API key, it was a really simple setup. *Note: I'm working with React.*

Step 1: Inject Google Places API script into the `webpack.config.js` file

```js
plugins: [
	...
    new HtmlWebpackPlugin({
      scripts: [
        "https://maps.googleapis.com/maps/api/js?key=API_KEY&libraries=places",
        {'async': true},
        {'defer': true}
      ]
    }),
    ...
]
```

Step 2: Create the `input` element

```html
<input type="text" id="location" placeholder="Location (Brooklyn, NY, USA)" />
```

Step 3: Bind the Autocomplete widget to the `input` element

```js
const input = document.getElementById("location");
const options = { types: ["(regions)"] };
this.locationSearch = new google.maps.places.Autocomplete(input, options);
```

Step 4: Add event listener to detect place changes

```js
this.locationSearch.addListener("place_changed", this.handleLocationSearch);
```

...and voilà!

<img src="http://i.imgur.com/I5ltGd7.gif">

So, what's the big headache?

Basically, it is to make the search box use the first suggestion from the autocomplete list when the user presses enter.

After much googling, a lot of people seemed to have the same issue. In fact, questions on [StackOverflow][3] dates all the way back to [2011][4]! Even until today, Google developers haven't implemented this feature.

There were tons of the same hacky solution like this [one][5], but none of them worked for me. So, I dug a little deeper and found [this][6] solution from the same thread. It wasn't the accepted answer nor the perfect answer, but it was a less hacky solution that steered me in the right direction.

Let's break down the code:

If the user selects a suggestion from the list, we can proceed with the search right away. Programmatically,

```js
if (location.address_components) {
  locationObj = parseGooglePlaces(location);
  this.props.dispatch(addLocationFilter(locationObj));
}
```

Otherwise, we need to retrieve the list of suggestions using the [AutocompleteService API][7] (not to be confused with Autocomplete) and parse out the details of the first result.

```js
const autocompleteService = new google.maps.places.AutocompleteService();
autocompleteService.getPlacePredictions({
  input: location.name,
  types: ["(regions)"]
}, (predictions, status) => {
  console.log(predictions);
});
```

The `predictions` response object should be the same list of suggestions that the user saw as they typed in the search box. We can use it to get the first suggestion, and it will be as if the user had selected it themselves. However, the object specification differs from the original. To retrieve the details of the location, we can use the `place_id` field to make a call to the [PlacesService API][8] `getDetails()` method.

```js
// The PlacesService expects as an parameter either a map or a node (HTML
// element) to render the attributions for the results.
const placesService = new google.maps.places.PlacesService(document.createElement('div'));
placesService.getDetails({
  placeId: predictions[0].place_id
}, (place, status) => {
  console.log(place);
});
```

The `place` response object contains the same exact data as the one retrieved from Autocomplete.

Success!!  🎉 🎉

Click [here][9] to see the full code.


[1]: https://developers.google.com/maps/documentation/javascript/places#place_searches
[2]: https://developers.google.com/maps/documentation/javascript/examples/places-autocomplete
[3]: https://stackoverflow.com/
[4]: https://stackoverflow.com/questions/7865446/google-maps-places-api-v3-autocomplete-select-first-option-on-enter
[5]: https://stackoverflow.com/a/11703018/5500643
[6]: https://stackoverflow.com/a/17505006/5500643
[7]: https://developers.google.com/maps/documentation/javascript/examples/places-queryprediction
[8]: https://developers.google.com/maps/documentation/javascript/places#place_details_requests
[9]: https://gist.github.com/sharynneazhar/770edfc360a5a4ccbf1c337d54d4160e
