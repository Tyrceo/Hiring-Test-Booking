# Tyrceo Hiring Test Booking

## The Story
We are looking for a better understanding of hotels in Spain based on public information from Booking.com. So, we have collected data for every hotel from four different regions in Spain:
  - Madrid
  - Barcelona
  - Costa del Sol
  - Costa Blanca.

We want to do a preliminary data visualization and explore the hotels and their reviews in these regions.

## Instructions
Your job is:
  1. to prepare the data:
      - **query** these results from the database
      - **filter data**: since the information from some hotels is out of date, we want to keep only the most recent reviews from the most active hotels on the booking platform:
          - only keep reviews from 2018 and 2019
          - only keep hotels with at least 5 reviews in 2019
      - **add geographic information**: we only have hotel addresses in the database but to visualize them on a map we need their coordinates (latitude/longitude):
          - you'll need to "[geocode](https://en.wikipedia.org/wiki/Address_geocoding)" these addresses
          - the resulting coordinates will allow you to generate a **geojson**
          - Note: you can visualize geojson using [mapshaper](https://mapshaper.org/) or [geojson.io](http://geojson.io/)
  2. to visualize the data:\
    using the prepared data:
      - build a simple html page with a mapbox **map** and a basic **table** to show the reviews
      - add a selector to filter between the four different regions or to visualize them all at once
          - show hotels from the selected region and hide the rest
          - update review table according to the set of selected hotels
      - bonus#1: when you click on or hover over a hotel, show a popup including basic information
      - bonus#2: set a navigate-to funcionality to improve UX when changing the areas from the selector
      - bonus#3: feel free to innovate and show your strengths

## Database
### Host and Credentials
The information to access the database should be included in the mail we sent you.

### Schema
```sql
CREATE TABLE `hotel_info` (
  `country_area` varchar(45) NOT NULL,
  `hotel_id` varchar(32) NOT NULL,
  `hotel_name` text,
  `hotel_url` text,
  `hotel_address` text,
  `review_score` decimal(5,2) DEFAULT NULL,
  `review_qty` int(11) DEFAULT NULL,
  `clean` decimal(5,2) DEFAULT NULL,
  `comf` decimal(5,2) DEFAULT NULL,
  `loct` decimal(5,2) DEFAULT NULL,
  `fclt` decimal(5,2) DEFAULT NULL,
  `staff` decimal(5,2) DEFAULT NULL,
  `vfm` decimal(5,2) DEFAULT NULL,
  `wifi` decimal(5,2) DEFAULT NULL,
  PRIMARY KEY (`hotel_id`,`country_area`),
  KEY `index2` (`hotel_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8


CREATE TABLE `hotel_reviews` (
  `UUID` varchar(36) DEFAULT NULL,
  `hotel_id` varchar(32) DEFAULT NULL,
  `review_title` text,
  `review_url` text,
  `review_score` decimal(5,2) DEFAULT NULL,
  `review_date` date DEFAULT NULL,
  `reviewer_name` text,
  `hash_reviewer_name` varchar(200) DEFAULT NULL,
  `reviewer_location` varchar(100) DEFAULT NULL,
  `posting_conts` int(11) DEFAULT NULL,
  `positive_content` text,
  `negative_content` text,
  `tag_n1` text,
  `tag_n2` text,
  `tag_n3` text,
  `tag_n4` text,
  `tag_n5` text,
  `staydate` text,
  KEY `index1` (`hotel_id`),
  KEY `index2` (`review_score`,`review_date`,`hotel_id`),
  KEY `index3` (`hash_reviewer_name`),
  KEY `index4` (`posting_conts`),
  KEY `index6` (`hotel_id`,`review_date`,`hash_reviewer_name`),
  KEY `index7` (`posting_conts`,`reviewer_location`,`hash_reviewer_name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
```
### Description
Data is stored in two separates table:

**`hotel_info`**\
Contains overview information about each hotel, including name, address, and review scoring (`review_score`) for the hotel itself and 7 different categories:
  - Cleanliness (clean)
  - Comfort (comf)
  - Location (loct)
  - Facilities (fclt)
  - Staff (staff)
  - Value for money (vfm)
  - Free WiFi (wifi).

**`hotel_reviews`**\
Contains the reviews associated to each hotel, including review date, scoring and information about the reviewer among others things.


## What is Geocoding?
> **Address geocoding**, or simply **geocoding**, is the process of taking a text-based description of a location, such as an address or the name of a place, and returning geographic coordinates, frequently latitude/longitude
[source](https://en.wikipedia.org/wiki/Address_geocoding)

Example:
```
Input:
hotel_address: '11 Calle de Esparteros Floor 4, Door 4, Centro de Madrid, 28012 Madrid'

Output:
longitude: -3.7053244194338273
latitude: 40.41544007462682
```

You can use any geocoding API you like.

## Mapbox
We generated a temporary token for you to use mapbox without having to create an account.\
It should have been included in the mail we sent you.\
Make sure you use [mapbox-gl-js](https://docs.mapbox.com/mapbox-gl-js/api/), and not the older mapbox.js

## Submission
You should at least submit the following files:
  - Python:
    - a Pipefile and Pipfile.lock or requirements.txt for your python dependencies
    - the python code you used for the preparation phase (it can be a jupyter notebook)
  - Data:
    - the geojson file you generated and you should be using in the visualization
  - HTML/Javascript/CSS:\
    the visualization itself
    - An html file for the data vizualization
    - optionally: the js and css if you keep them separated

Basically we should be able to easily:
  - reproduce your python environment (`pipenv install` or `pip install -r requirements.txt`) and run your code to generate the geojson
  - as well as see your final visualization.

Once you're done with the assignement, the preferred submission method is by
sending us a link to a public repository where you've committed your solution (github, gitlab, bitbucket, etc).\
We also accept the aforementionned files zipped in a mail attachment.

Send your submission at <desarrollo@tyrceo.com>


## Suggested python version and libraries
You are free to use any libraries you want.\
You are expected to use python 3 syntax (we use 3.7+ these days).\
You'll probably find the following libraries quite useful:
  - pandas and geopandas
  - pymysql

## Local http server
You'll probably need to run a local server when developping the visualization.\
You are also free to choose any method you want.\
If you're not sure, running the following terminal command, in the directory of your html file should suffice.
```sh
python -m http.server
```

## Final words
The few problems you'll encounter in this test are basic data filtering, use of external API and data visualization tasks we regularily have to work on.
So the goal of this test is two-fold:
  1. giving us an idea of how you can deal with technology or problem you're not used to deal with
  2. introducing you to some of the tools you'll be using often when working at Tyrceo (python, pandas, mapbox, database, etc.)

If you have any question/doubt don't hesitate to send a mail at <desarrollo@tyrceo.com>

Good luck and have fun.
