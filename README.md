# RatyRate Stars Rating Gem

A Ruby Gem that wrap the functionality of [jQuery Raty](https://github.com/wbotelhos/raty) library, and provides optional IMDB style rating.

[![Gem Version](https://badge.fury.io/rb/ratyrate.svg)](http://badge.fury.io/rb/ratyrate)
[![Build Status](https://travis-ci.org/wazery/ratyrate.svg)](http://travis-ci.org/wazery/ratyrate)
[![Dependency Status](https://gemnasium.com/wazery/ratyrate.svg)](https://gemnasium.com/wazery/ratyrate)
[![Code Climate](https://codeclimate.com/github/wazery/ratyrate.png)](https://codeclimate.com/github/wazery/ratyrate)
[![License](http://img.shields.io/license/MIT.png?color=green)](http://opensource.org/licenses/MIT)
[![Support jQuery Raty](http://img.shields.io/gittip/wbotelhos.svg)](https://www.gittip.com/wazery "Git Tip")
[![Stories in Ready](https://badge.waffle.io/wazery/ratyrate.png?label=ready&title=Ready)](https://waffle.io/wazery/ratyrate)
[![wazery/ratyrate API Documentation](https://www.omniref.com/github/wazery/ratyrate.png)](https://www.omniref.com/github/wazery/ratyrate)

## Repository

This is a fork against the repository [muratguzel/letsrate](https://github.com/muratguzel/letsrate), the aim of this fork is to refresh the development in this Gem with a new scope and features, so please if you have any pull request issue it here, also I imported all the issues in the original repo.

## Changelog from the main repository

1. Exposed a lot of jQuery Raty plugin [features](http://wbotelhos.com/raty)
  1. Added cancel button that can be customized and inserted to left or right
  2. Added the ability to set custom star images for on, half or off state
  3. Added the ability to set a custom path for the images
  4. Added the ability to enable/disable half stars
  5. Added the ability to turn on/off half star
2. Their is a new option now to disable or enable the rating after the first rate
3. Added a star style to show just the score of the dimension, but this star is not editable
4. Added an overall average star just like IMDB style
5. Created a Heroku app to illustrate this Gem's purpose and features (MovieStore)
6. [Wrote a complete tutorial on SitePoint that illustrates how to use this gem](http://www.sitepoint.com/ratyrate-add-rating-rails-app/)
3. Nothing else
4. :wq

## TODO

1. Write RSpec tests for this Gem
2. Add a share helper to Facebook, Twitter
3. Force refresh after rating when ***disable_after_rate*** and ***imdb_avg*** options is set to true.

## Detailed view of the new features

![Detailed view of the new featurews](https://dl.dropboxusercontent.com/u/71605080/RatyRate%20Features.png)

## Complete tutorial for this Gem on SitePoint - Ruby

I wrote a complete tutorial on SitePoint to cover this Gem's features in detail, if you are concerned to learn more about it just pay a visit to this [link](http://www.sitepoint.com/ratyrate-add-rating-rails-app/).

## Instructions

### Install

Add the ratyrate gem into your Gemfile

```ruby
gem 'ratyrate'
```

### Generate

```
rails g ratyrate user
```

The generator takes one argument which is the name of your existing devise user model UserModelName. This is necessary to bind the user and rating datas.
Also the generator copies necessary files (jQuery Raty plugin files, star icons and JavaScript files)

Example:

Suppose you will have a devise user model which name is User. The devise generator and ratyrate generator should be like below

```
rails g devise:install
rails g devise user

rails g ratyrate user # => user is the model generated by devise
```

This generator will create Rate and RatingCache models,
db/migrations,
and link to your user model.

### Database Migration

Run the migrations:
```
rake db:migrate
```

### Javascript include
```
//= require jquery.raty
//= require ratyrate
```

### Prepare

I suppose you have a car model

```
rails g model car name:string
```

You should add the ratyrate_rateable function with its dimensions option. You can add multiple dimensions.

```ruby
class Car < ActiveRecord::Base
  ratyrate_rateable "speed", "engine", "price"
end
```

Then you need to add a call ratyrate_rater in the user model.

```ruby
class User < ActiveRecord::Base
  ratyrate_rater
end
```

### Using

There is a helper method which name is rating_for to add the star links. By default rating_for will display the average rating and accept the
new rating value from authenticated user.

```erb
<%# show.html.erb -> /cars/1 %>

Speed  : <%= rating_for @car, 'speed' %>
Engine : <%= rating_for @car, 'engine' %>
Price  : <%= rating_for @car, 'price' %>
```

### Available Options

1- If you need to change the star number, you should use star option like below.
```erb
Speed  : <%= rating_for @car, 'speed', star: 10 %>
Engine : <%= rating_for @car, 'engine', star: 7 %>
Price  : <%= rating_for @car, 'price' %>
```
2- If you want to disable/enable the rating after user's first rate use the new option *disable_after_rate*
```erb
Speed : <%= rating_for @car, 'speed', disable_after_rate: true %>
```
To enable changes after first user rate set ```disable_after_rate``` to false

3- To enable half stars use the option *enable_half*
```erb
Speed : <%= rating_for @car, 'speed', enable_half: true %>
```
4- To show or hide the half stars use *half_show*
```erb
Speed : <%= rating_for @car, 'speed', half_show: true %>
```
5- To change the path in which the star images (star-on.png, star-off.png, star-half.png, ..etc) are located use
```erb
Speed : <%= rating_for @car, 'speed', star_path: true %>
```

To just change one of the star images choose from these options (star_on, star_off, star_half)

6- To add the cancel button to the left, or right of the stars use **(default is false)**
```erb
Speed : <%= rating_for @car, 'speed', cancel: true %>
```
7- To change the place of the cancel button (left, or right) use **(default is left)**
```erb
Speed : <%= rating_for @car, 'speed', cancel_place: left %>
```
8- To change the hint on the cancel button use **(default is "Cancel current rating!" )**
```erb
Speed : <%= rating_for @car, 'speed', cancel_hint: 'Cancel this rating!' %>
```
9- To change the image of the cancel on button use
```erb
Speed : <%= rating_for @car, 'speed', cancel_on: 'cancel-on2.png' %>
```
10- To change the image of the cancel off use
```erb
  Speed : <%= rating_for @car, 'speed', cancel_off: 'cancel-off2.png' %>
```
11- To show the number of voters **(default is false)**
```erb
Speed : <%= rating_for @car, 'speed', num_of_voters: true %>
```

### Other Helpers

You can use the *rating_for_user* helper method to show the star rating for the user.

```erb
Speed : <%= rating_for_user @car, current_user, 'speed', star: 10 %>
```

And you can use the *imdb_style_rating_for* to show a similar to IMDB rating style.

```erb
Speed : <%= imdb_style_rating_for @car, current_user %>
```

## Semantic Versioning

Ratyrate attempts to follow semantic versioning in the format of `x.y.z`, where:

`x` stands for a major version (new features that are not backward-compatible).

`y` stands for a minor version (new features that are backward-compatible).

`z` stands for a patch (bug fixes).

In other words: `Major.Minor.Patch`.

## Feedback
If you find bugs please open a ticket at [https://github.com/wazery/ratyrate/issues](https://github.com/wazery/ratyrate/issues)

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/wazery/ratyrate/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
