# MapboxDirections

[![Build Status](https://travis-ci.org/allyapp/mapbox_directions.svg?branch=master)](https://travis-ci.org/allyapp/mapbox_directions)
[![Code Climate](https://codeclimate.com/github/allyapp/mapbox_directions/badges/gpa.svg)](https://codeclimate.com/github/allyapp/mapbox_directions)

Ruby wrapper for the MapBox Directions Service.

Here you can find the documentation of the API interface:
https://www.mapbox.com/developers/api/directions/

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'mapbox_directions'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install mapbox_directions

## Usage

Here's the list of supported parameters and the possible values. Note that parameters that are not optional are required:

- **access_token**: public token provided by MapBox.
- **mode**: mode of transport applied to process the routing.
Values: ``driving``, ``cycling``or``walking``.
- **origin**: origin decimal coordinates where the route starts.
Format: ``"#{lng},#{lat}"``
- **destination**: destination decimal coordinates where the route ends.
Format: ``"#{lng},#{lat}"``
- **geometry**(optional): format for route geometry.
Values ``geojson``(default), ``polyline``and ``false`` to omit geometry.
- **alternatives**(optional): whether to get more than one route as an alternative or not.
Values: ``true``(default) or ``false`` as a Boolean.
- **instructions**(optional): format for route instructions.
Values: ``text``(default) or ``html``.


```ruby
require "mapbox_directions"

parameters = {
  access_token: "<your_access_token>",
  mode:         "driving",       # %w(driving cycling walking)
  origin:       "-122.42,37.78", # "#{lng},#{lat}"
  destination:  "-77.03,38.91",  # "#{lng},#{lat}"
  geometry:     "polyline",      # %w(polyline geojson false)
  alternatives: false,           # true or false
  instructions: "text",          # %w(text html)
}
MapboxDirections.directions(parameters)
```



### Exceptions

Exceptions are triggered to give immediate feedback when there's something wrong in the parameters passed that won't allow to get a successful response.

- **``MapboxDirections::MissingAccessTokenError``**: When the access token is not passed in the parameters or its value is ``nil``


- **``MapboxDirections::InvalidAccessTokenError``**: When the access token is passed in the parameters but it's invalid

- **``MapboxDirections::CoordinatesFormatError``**: When the decimal coordinates are passed bad formatted given the requirements.


- **``MapboxDirections::UnsupportedTransportModeError``**: When the mode of transport is different that the ones that the service supports.


If the exception raise is an undesired behaviour in the context where the code is being executed, they can be rescued one by one or all at once follows:
```ruby
require "mapbox_directions"

def directions(parameters)
  MapboxDirections.directions(parameters)
rescue MapboxDirections::Error
  # Handle exception
end
```

## Contributing

1. Fork it ( https://github.com/[my-github-username]/mapbox_directions/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
