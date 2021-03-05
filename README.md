# Tyrceo Hiring Test Booking

## The Story
We are looking for a better understanding of hotels in Spain based on public information from Booking.com. So, we have collected data for every hotel from four different regions in Spain: Madrid, Barcelona, Costa del Sol and Costa Blanca. We want to do a preliminary data visualization and explore the hotels and reviews in these regions.

## Instructions
Your job is:
  - to prepare the data:
    - **query** these results from the database
    - **filter data**
      - information from some hotels is out of date
      - we want to keep only most recent reviews from most active hotels on the booking platform
      - only keep reviews from 2018 and 2019
      - only keep hotels with at least 5 reviews in 2019
    - **geographic information**:
      - we have hotel address in the database but we are missing coordinates (lat, lng)
      - so we need to geocoding these addresses 
      - once geocoding is done, you can export this data as a **geojson**
      - Note: you can visualize the result using [mapshaper](https://mapshaper.org/) or [geojson.io](http://geojson.io/)      
  - to visualize the data:
    - build a simple html page with a mapbox map and a basic table to show the reviews
    - add a selector to change between the four different regions or to visualize all of them
      - show hotels from the selected region and hide the rest 
      - update review-table according to the set of hotels selected
    - add the geojson you prepared as a source
    - bonus#1: when you click on or hover over a hotel, show a popup including basic information
    - bonus#2: set a navigate-to funcionality to improve UX when changing the areas from the selector
    - bonus#3: feel free to innovate and show your strengths

## Database Structure
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

### Geocoding
Address geocoding, or simply geocoding, is the process of taking a text-based description of a location, such as an address or the name of a place, and returning geographic coordinates, frequently latitude/longitude [https://en.wikipedia.org/wiki/Address_geocoding]:

```
Input:
hotel_id: '00129a71268e171231920d283f327359'
hotel_name: 'Unbeatable Sol Mayor APT'
hotel_address: '11 Calle de Esparteros Floor 4, Door 4, Centro de Madrid, 28012 Madrid,'

Output:,
longitude: '-3.7075158,17'
latitude: '40.4154105'
```

You could use whatever geocoding api you prefer. 

### Database design
Data is easily store in two separates table. 

First one, **hotel_info**, contains overview information about each hotel, including name, address, and review scoring for the hotel itself (review_score) and 7 different categories:
Cleanliness (clean), Comfort (comf), Location (loct), Facilities (fclt), Staff (staff), Value for money (vfm) and Free WiFi (wifi). 

Second one, **hotel_review**, contains the reviews associated to each hotel, including review date, scoring and information about the reviewer among others.


## Mapbox
We generated a temporary token for you to use mapbox without having to create an account
It should have been included in the mail we sent you.\
Make sure you use [mapbox-gl-js](https://docs.mapbox.com/mapbox-gl-js/api/), and not the older mapbox.js

## Submission
You should at least submit the following files:
  - a Pipefile and Pipfile.lock or requirements.txt for your python dependencies
  - a python script for the data preparation and geocoding 
    - or optionally a jupyter notebook
  - an html file for the data vizualization
    - optionally separated css/js or you can keep everything into one html file
  - a geojson file resulting from executing the python script and accessible from the html

Once you're done with the assignement, the preferred submission method is by
sending us a link to a public repository where you've committed your solution (github, gitlab, bitbucket, etc).\
We also accept the aforementionned files zipped in a mail attachment.\
Send your submission at **desarrollo@tyrceo.com**


## Suggested python version and libraries
You are free to use any libraries you want.\
You are expected to use python 3 syntax (we use 3.7.5 these days).\
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

If you have any question/doubt don't hesitate to send a mail at desarrollo@tyrceo.com

Good luck and have fun.
