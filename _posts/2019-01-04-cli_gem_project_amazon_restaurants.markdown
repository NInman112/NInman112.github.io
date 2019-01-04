---
layout: post
title:      "CLI Gem Project (Amazon Restaurants)"
date:       2019-01-04 19:36:53 +0000
permalink:  cli_gem_project_amazon_restaurants
---


This project had a couple of transformations, initially it was intended to scrape Ubereats but their website ended up not being scraping friendly.  The next selection was DoorDash but it also was not capable of being scraped.  A quick check of Amazon revealed that they use meaningful and understandable class names and it is scraping friendly!  

The original idea was to allow the user to enter in a zip code and then show available restaurants near them, if the zip provided retrieved nothing, it would then prompt to try another.  Searching by zip was not working so that changed to focusing on major cities as well as cuisine types.  The restaurants and cuisine results were limited to ten since most major cities have a massive amount of restaurants and scraping that much data would take a good chunk of time.

On startup, the CLI application preloads cities and cuisines:
```
  def start
    puts 'Find local restaurants available through Amazon Restaurants!'.colorize(:green)
    puts "Loading additional data....  please wait a moment..".colorize(:red)
    Scraper.new.scrape_cities
    Scraper.new.scrape_cuisines
    menu
  end
```

Once loaded the user is prompted to choose between cities and cuisines, both are scraped from Amazon Restaurants so if additional cities or cuisines are added, the CLI application should be able to retrieve them.  The city selection is done by number, the application will take in user input and display the first ten restaurants from the chosen city.  

```
  def city_select
    loop do
      Cities.print
      puts "Please make a selection, or enter '0' to go to the menu:".colorize(:red)
      input = gets.chomp
      name = Cities.all[input.to_i - 1].name.downcase

      if name == "los angeles and orange county, ca"
        name = "los-angeles, zz"
      elsif name == "bay area, ca"
        name = "san-francisco"
      elsif name == "manhattan and brooklyn, ny"
        name = "new-york, zz"
      elsif name == "washington, d.c. area"
        name = "washington-dc, zz"
      end
      case input.to_i
      when 1..20
        puts "Restaurants in the #{Cities.all[input.to_i - 1].name} area: ".colorize(:blue)
        Restaurant.print_details(name.split(", ")[0])
      when 0
        menu
      end
    end
  end
```

One issue that was encountered is that a few of the city names scraped from the site were different and could not be used in the URL.  One example from above is "bay area, ca" which is one of the selections available.  Bay area doesn’t work when put into the URL, instead the URL wanted san-francisco:

https://www.amazon.com/restaurants/delivery/san-francisco

A few of the cuisines also encountered the same issue as above so they also have to run through a conditional in order to be added to the URL.  There are additional features that could be implemented that would make it more useful, being able to search by zip is one.  Printing out additional restaurants beyond the first 10 would also add some helpful features.


