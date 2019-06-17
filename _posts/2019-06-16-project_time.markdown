---
layout: post
title:      "Project Time..."
date:       2019-06-17 01:57:22 +0000
permalink:  project_time
---


![](https://media.giphy.com/media/iGXCvFetkxmS95ocWO/giphy.gif)

Time to use all of the tools we've learned thus far and make our very own CLI gem!

The CLI Gem must: 
* provide access to data from a webpage via scraping 
* go two levels deep 
* use good OO patterns 

Immediately, I knew I wanted to make a lightsaber builder!

![](https://media.giphy.com/media/l0IpYf36OQy9O7ENW/giphy.gif)

It would allow for the user to choose a side (light vs. dark duh!) and then build their lightsaber via different colors and hilts styles. Alas, I could not find a website for scraping that would allow me to design said CLI and scrape two levels deep. 

Me trying to scrape Wookieepedia and force it to fit the project requirements:

![](https://media3.giphy.com/media/kByr3QIp3RWjm/giphy.gif?cid=790b76115d05b7a97752537177874ac5&rid=giphy.gif)![](https://media3.giphy.com/media/OcBHX3u8GOO0E/giphy.gif?cid=790b76115d05b7c34e6e456573f0a3e0&rid=giphy.gif)![](https://media3.giphy.com/media/10zhtkpqSffMiI/giphy.gif?cid=790b76115d05b82c4f444f5367b81eea&rid=giphy.gif)

A passion project for another day... ;)


So brainstorm #2 led me to a World's Best Tiki Bar finder because why not!

When  my fiance Ryan and I travel, one of the first things we look for are any local tiki bars. He is an unofficial-official Mai Tai aficionado. This one's for you babe. 

Essentially, my CLI provides the user with a list of the top 15 tiki bars in the world (as determined by the discerning people of PUNCH) and lets them choose a bar to learn more about. 

## Scraping

Scraping is an art, and the more you do the more it starts to make sense. It is made possible with the help of the Nokogiri gem. I can't imagine the task of scraping without it, which is why the fact that it 's one of the most downloaded gems is entirely unsurprising. That is the level of world-changing, useful coding to aspire to!

The first round of the second-level scrape I did was pretty basic so once I accomplished it I knew I needed to do more. But the data I wanted was in a different place and I was struggling and that's how I landed myself here...

```
def self.scrape_bar_info(bar)
    html = Nokogiri::HTML(open(bar.url))
    deets = html.css("div.info-box")

    bar.address = deets.css("a")[0].text.strip if bar.address == nil 
    bar.hours = deets.css("div")[2].text.strip if bar.hours == nil
  end 
  
  def self.scrape_more_info(bar)
    more_info = {}
    
    html = Nokogiri::HTML(open(bar.url))
    more = html.css("div.bordered-box.clearfix")

    more.each do |i|
      if i.css("div.module h5")[2].text == "Neighborhood"
        more_info[:neighborhood] = i.css("a").text.strip
      elsif i.css("div.module h5").last.text == "ProTip"
        more_info[:protip] = i.css("span")[1].text
      end
    end 

    more_info[:what_to_drink] = more.css("span")[0].text.strip

    more_info
  end 
```

Not great, I know. The first scrape method is simply assigning the attributes. The second one is returning a hash which I then wrote out a method in the Bar class to assign the values from said hash. Why you do this brain? On the flip side, look how much room for improvement I left for myself??

![](https://media2.giphy.com/media/WxzjsygSlVd0o6hamp/giphy.gif?cid=790b76115d05be0a76786a446fd99db5&rid=giphy.gif)

### Refactoring

There was even more data I wanted to scrape as well and in doing so, I was able to seriously refactor the above code. 

```
def self.scrape_bar_info(bar)
    html = Nokogiri::HTML(open(bar.url))
    deets = html.css("div.left-col.col-xs-12.col-sm-12.col-md-8.col-lg-8")

    bar.address = deets.css("div.info-box a")[0].text.strip if bar.address == nil 
    bar.hours = deets.css("div.info-box div")[1].text.strip if bar.hours == nil
    bar.website = deets.css("div.info-box a")[1].attr('href') if bar.website == nil
    bar.what_to_drink = deets.css("div.module span")[0].text.strip if bar.what_to_drink == nil 
    bar.known_for = deets.css("div.module ul li").map(&:text) if bar.known_for == nil 
    
    if deets.css("div.module h5").text.include?("Neighborhood")
      bar.neighborhood = deets.css("div.module a").text.strip if bar.neighborhood == nil 
    end 
    if deets.css("div.module h5").text.include?("ProTip")
      bar.protip = deets.css("div.module span")[1].text.strip if bar.protip == nil 
    end
  end
```

Now my second-level scrape is contained to one class method! And I'm not returning half of my data in a hash anymore! The first time I call the #css method, it's scraping and returning the whole page in an array, and then each bar attribute is parsing further from there. I wasn't looking at scraping with a wide enough lens, thought it would be simpler the closer I started to the element I wanted. Absolutely not the case.  This refactoring also allowed me to remove an instance method from my Bar class and from my CLI class, DRY-ing up my code. 

It's Mai Tai o'clock! Cheers!

![](https://imgur.com/Rq0PaKU.gif)











