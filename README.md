# FieldtypeMapMarker Module for ProcessWire 

This Fieldtype for ProcessWire holds an address or location name, and automatically
geocodes the address to latitude/longitude using Google Maps API. The resulting 
values may be used to populate any kind of map (whether Google Maps or another). 

This Fieldtype was created to serve as an example of creating a custom Fieldtype and 
Inputfield that contains multiple pieces of data. Though the Fieldtype has now gone
far beyond that and is relatively full featured. As a result, it may no longer be
the simplest example of how to implement a Fieldtype/Inputfield, though it is very
effective and useful. 

MapMarker also has a corresponding Inputfield and Markup module, named
InputfieldMapMarker and MarkupGoogleMap. When you install FieldtypeMapMarker, the
Inputfield will also be installed and used for input on the admin side. Installation
of MarkupGoogleMap is optional. It provides a simple way to render Google maps with
the data managed by FieldtypeMapMarker. 

This Fieldtype has a support forum at: 
http://processwire.com/talk/index.php/topic,752.0.html

## Using Map Marker

### How to install

1. Copy all of the files for this module into /site/modules/FieldtypeMapMarker/

2. In your admin, go to the Modules screen and "check for new modules." Click *install*
   for the Map Marker Fieldtype. 

3. In your admin, go to Setup > Fields > Add New Field. Choose MapMarker as the type.
   If you are not sure what to name your field, simply "map" is a good one! Once created,
   configure the settings on the *input* tab. 

4. Add your new "map" field to one or more templates, as you would any other field. 

### How to use from the page editor

1. Create or edit a page using one of the templates you added the "map" field to. 

2. Type in a location or address into the "address" box for the map field. Then click 
   outside of the address, and the Javascript geocoder should automatically populate the
   latitude, longitude and map location. The Google geocoder will accept full addresses
   or known location names. For instance, you could type in "Disney Land" and it knows
   how to find locations like that. 

3. The geocoding also works in reverse. You may drag the map marker wherever you want
   and it will populate the address field for you. You may also populate the latitude,
   longitude and zoom fields manually if you like. Unchecking the box between address
   and latitude disables the geocoder. 

### How to use from the API, in your template files 

In your template files, you can utilize this data for your own Google Maps (or anything 
else that you might need latitude/longitude for). 

Lets assume that your field is called 'map'. Here is how you would access the
components of it from the API:

echo $page->map->address;	// outputs the address you entered
echo $page->map->lat; 		// outputs the latitude
echo $page->map->lng; 		// outputs the longitude
echo $page->map->zoom;		// outputs the zoom level

-------------

## Markup Google Map

This package also comes with a module called MarkupGoogleMap. It provides a simple means
of outputting a Google Map based on the data managed by FieldtypeMapMarker. To install,
simply click "install" for the Google Maps (Markup) module. This is a Markup module, 
meaning it exists primarily to generate markup for output on the front-end of your site.

### How to use

Add this somewhere before your closing `</head>` tag:
`````````
<script type='text/javascript' src='https://maps.googleapis.com/maps/api/js?sensor=false'></script>
`````````

In the location where you want to output your map, place the following in your template file:
`````````
$map = $modules->get('MarkupGoogleMap'); 
echo $map->render($page, 'map'); 
// $page is the Page object that has the 'map' field
// replace 'map' with the name of your FieldtypeMap field
`````````

To render a map with multiple markers on it, specify a PageArray rather than a single $page: 
`````````
$items = $pages->find("template=something, map!='', sort=title"); 
$map = $modules->get('MarkupGoogleMap'); 
echo $map->render($items, 'map'); 
`````````

To specify options, provide a 3rd argument with an options array:
`````````
$map = $modules->get('MarkupGoogleMap'); 
echo $map->render($items, 'map', array('height' => '500px')); 
`````````

### Options

Here is a list of all possible options (with defaults shown):  

- default width of the map  
  `width` => `100%`

- default height of the map
  `height` => `300px`

- zoom level
  `zoom` => `12` (or `$field->defaultZoom`)

- map type: ROADMAP, HYBRID or SATELLITE
  `type` => `HYBRID` or `$field->defaultType`

- map ID attribute
  `id` => `mgmap`

- map class attribute
  `class` => `MarkupGoogleMap`

- center latitude
  `lat` => `$field->defaultLat`

- center longitude
  `lng` => `$field->defaultLng`

- set to false only if you will style the map <div> yourself
`useStyles` => `true`

- allows single-marker map to use marker settings rather than map settings
`useMarkerSettings` => `true` 

- field to use for the marker link, or blank to not link
`markerLinkField` => `url`

- field to use for the marker title
`markerTitleField` => `title`

- map will automatically adjust to fit to the given markers (when multiple markers)
`fitToMarkers` => `true` 

---------

