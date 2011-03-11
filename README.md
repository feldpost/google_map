# GoogleMap

A RubyGem that and makes generating google maps easy as pie.

INSTALL
-------
In your `Gemfile`, add the following dependency:
    gem 'google_map'
Run:
    $ bundle install

CONFIGURATION
-------------

  Set your Google Maps API Key in environment.rb (or somewhere else if you'd prefer)
	I'd suggest copying the configuration code out of your environment.rb and into an initializer named geokit
  This key is good for localhost:3000, signup for more at http://www.google.com/apis/maps/signup.html
  
  GOOGLE_APPLICATION_ID = "ABQIAAAA3HdfrnxFAPWyY-aiJUxmqRTJQa0g3IQ9GZqIMmInSLzwtGDKaBQ0KYLwBEKSM7F9gCevcsIf6WPuIQ"

# Usage

MAP CONTROLS
------------

maps_controller.rb
--------------------------

    class MapsController < ApplicationController
    	def	show
    		@map = GoogleMap::Map.new
	
    		# define control types shown on map
    		@map.controls = [ :large, :scale, :type ]
    		# valid controls options include
    		# :large 
    		# :small 
    		# :overview
    		# :large_3d
    		# :scale
    		# :type
    		# :menu_type
    		# :hierachical_type
    		# :zoom
    		# :zoom_3d
    		# :nav_label
        	
    		# allow user to double click to zoom
    		@map.double_click_zoom = true
	
    		# not certain what this does
    		@map.continuous_zoom = false
	
    		# allow user to scroll using mouse wheel?
    		@map.scroll_wheel_zoom = false
	
    	end
    end

MAP CENTERING AND ZOOM
----------------------

maps_controller.rb
------------------

    class MapsController < ApplicationController
    	def	show
    		@map = GoogleMap::Map.new
    		@map.center = GoogleMap::Point.new(47.6597, -122.318) #SEATTLE WASHINGTON
    		@map.zoom = 10 #200km
    	end
    end

MAP CENTERING USING BOUNDS
--------------------------

maps_controller.rb
--------------------------
    class MapsController < ApplicationController
    	def	show
    		@map = GoogleMap::Map.new
    		@map.bounds =  [GoogleMap::Point.new(47.6597, -121.318), GoogleMap::Point.new(48.6597, -123.318)] #SEATTLE WASHINGTON 50KM
    	end
    end


SIMPLE MARKER USAGE
-------------------

maps_controller.rb
--------------------------
    class MapsController < ApplicationController
    	def	show
    		@map = GoogleMap::Map.new
      		@map.markers << GoogleMap::Marker.new(	:map => @map, 
                                         			:lat => 47.6597, 
                                         			:lng => -122.318,
                                         			:html => 'My House')
    	end
    end

maps/show.html.erb
-------------------------
    <%= @map.to_html %>
    <div style="width: 500px; height: 500px;">
      <%= @map.div %>
    </div>


Advanced Marker Usage
---------------------

Available icon classes:
* GoogleMap::LetterIcon.new(@map, 'A') # letter must be uppercase
* GoogleMap::SmallIcon.new(@map, 'yellow')

maps_controller.rb
--------------------------
    class MapsController < ApplicationController
    	def	show
    		@map = GoogleMap::Map.new
      		@map.markers << GoogleMap::Marker.new(	:map => @map, 
    												:icon => GoogleMap::SmallIcon.new(@map, 'blue'),
    												:lat => 47.6597, 
    												:lng => -122.318,
    												:html => 'My House',
    												:marker_icon_path => '/path/to/image',
    												:marker_hover_text => 'String to show on Mouse Over',
    												:open_infoWindow => true #opens marker by default
    												)
  
    	end
    end

maps/show.html.erb
-------------------------
    <%= @map.to_html %>
    <div style="width: 500px; height: 500px;">
      <%= @map.div %>
    </div>


PLOTTING POLYLINE ROUTES
------------------------

maps_controller.rb
------------------
    class MapsController < ApplicationController
    	def	show
    		@map = GoogleMap::Map.new
  		
    		# plot points for polyline
            vertices = []
            object.gpxroute.gpxtrackpoints.each do |p|
              vertices << GoogleMap::Point.new(p.lat, p.lon)
            end
		
      		# plot polyline
    		@map.overlays << GoogleMap::Polyline.new(	:map => @map, 
    													:color=>'#FF0000', 
    													:weight=>'2', 
    													:opacity=>'.5', 
    													:vertices=>vertices
    													)
    	end
    end

maps/show.html.erb
-------------------------
    <%= @map.to_html %>
    <div style="width: 500px; height: 500px;">
      <%= @map.div %>
    </div>
