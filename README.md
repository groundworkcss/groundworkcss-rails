GroundworkCSS Rails Gem
====


Add GroundworkCSS to your rails project.


Add this to your Gemfile:
```
gem 'groundworkcss', :git => 'https://github.com/groundworkcss/groundworkcss-rails'
```

And then run:
```
$ bundle exec rails g groundworkcss:install
```

Yay!


# Installation


Inside your Gemfile add the following line:

```ruby
group :assets do
  gem 'groundworkcss', :git => 'git://github.com/groundworkcss/groundworkcss-rails.git'
end
```

Then run `bundle install` to install the gem.

## And then?

If you want to include GroundworkCSS on all of your pages then run the following to append `groundworkcss` to your application sprockets files:

```bash
rails g groundworkcss:install
```

You can also manually include `groundworkcss` on individual pages:

```ruby
stylesheet_link_tag "groundworkcss"
javascript_include_tag "groundworkcss"
```

Or add `require "groundworkcss"` to your sprockets file(s):

**in application.css**  

```css
/*= require "groundworkcss" */
```

**in application.js**  

```javascript
//= require "groundworkcss"
```

### Advanced

Alternatively, you may be more selective about which modules to use and can selectively add stylesheets and javascripts to your project:

**in application.css**

```css
/*
*= require "groundworkcss/core/all"
*= require "groundworkcss/base/all"
*= require "groundworkcss/type/all"
*/
```

**in application.js**

```javascript
//= require "groundworkcss/jquery.cycl2.js"
```

### Setup your layout with the Groundwork boilerplate

Add the following to the `<head>` section in your layout file (i.e. `app/views/layouts/application.html.erb`)

```html
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=1" />
  <!-- Modernizr -->
  <script src="#{asset-path("libs/modernizr-2.6.2.min.js", javascript)}"></script>
  <!-- framework css -->
  <!--[if IE]>
  <link type="text/css" rel="stylesheet" href="/css/groundwork-ie.css">
  <![endif]-->
  <!--[if lt IE 9]>
  <script type="text/javascript" src="/js/libs/html5shiv.min.js"></script>
  <![endif]-->
  <!--[if IE 7]>
  <link type="text/css" rel="stylesheet" href="/css/font-awesome-ie7.min.css">
  <![endif]-->
  <script type="text/javascript">
    // fallback if SVG unsupported
    Modernizr.load({
      test: Modernizr.inlinesvg,
      nope: [
        '#{asset-path("no-svg.css")}'
      ]
    });
    // polyfill for HTML5 placeholders
    Modernizr.load({
      test: Modernizr.input.placeholder,
      nope: [
        '#{asset-path("placeholder_polyfill.css")}',
        '#{asset-path("libs/placeholder_polyfill.jquery.js", javascript)}'
      ]
    });
  </script>
```


# Using GroundworkCSS in production

Before pushing your application to production, you'll need to determine how you want your assets served.  You can either compile them on-the-fly or precompile them (recommended).  Before continuing you'll want to check your Rails application Gemfile and ensure that `gem 'groundworkcss', :git => 'git://github.com/groundworkcss/groundworkcss-rails.git'` is included in the `assets` group.

## To compile on-the-fly

In your Rails application edit `config/application.rb` and change:

```ruby
Bundler.require(*Rails.groups(:assets => %w(development test)))
```

to:

```ruby
Bundler.require(:default, :assets, Rails.env)
```

Then in `config/environments/production.rb` make sure the following setting exists:

```ruby
config.assets.compile = true
```

Assets will be compiled on the first visits to your site, which can cause a delay.

## To precompile assets using Capistrano

In your Rails application edit `Capfile` and add the following line after `load 'deploy'`:

```ruby
load 'deploy/assets'
```

Now when you run `cap deploy` the `deploy:assets:precompile` task will be run which takes care of running `bundle exec rake assets:precompile` for you.

By default assets that are precompiled will be located in the `public/assets/` folder of your application.


# Dependencies

* rails (~> 3.1)
* jquery-rails (~> 1.0)
