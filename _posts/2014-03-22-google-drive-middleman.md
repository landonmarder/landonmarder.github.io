---
layout: post
title: Using Google Spreadsheets with Middleman as a faux backend
categories: posts
date: 2014-03-22 11:00:00
summary: Middleman is a great tool to create a static website, but still have some things that you can take for granted with the Rails framework. Namely, you can use Ruby, ERB tags, SASS, and you can deploy quickly to heroku. Also, a big strength (but also a weakness) of Middleman is that it limits the extent of what you can do, so it makes sure that what you are doing is purposeful so that you do not create technical debt.
---

[Middleman](http://middlemanapp.com/) is a great tool to create a static website, but still have some things that you can take for granted with the Rails framework. Namely, you can use Ruby, ERB tags, SASS, and you can deploy quickly to heroku. Also, a big strength (but also a weakness) of Middleman is that it limits the extent of what you can do, so it makes sure that what you are doing is purposeful so that you do not create technical debt.

However, something that frustrates me about using Middleman is that when you are making lists of things, your HTML gets very repetitive because you have to write the HTML for each item in the list, there is no database to get data from.  This makes adding and subtracting list items cumbersome and repetitive. Also another frustration is that you cannot create an easy to use admin interface for programmers to interact with because Middleman sites are static.

To solve this dual problem, I first read [this documentation](http://middlemanapp.com/advanced/local-data/) from Middleman. With Middleman, you can store data as either yaml or json in a data directory in your Middleman app and access this in your HTML through a traditional Ruby loop using ERB. This eliminates the repetition of writing HTML for each list item and creates a local backend that your source code can use before it gets built.

Knowing that if I were able to get my data structured into json I could access it via Middleman (and thus would build my HTML without the need for me to be repetitive), I set out to look for a solution for a more user friendly way of inputting and editing the data list. I found [this solution](https://github.com/gimite/google-drive-ruby), which allows you to read data from a google drive spreadsheet using Ruby. You can read the google spreadsheet and convert the data into json using a rake task, which you can then write into the appropriate file in the data directory of your Middleman app.

Thus, a non-programmer can update the data list using a spreadsheet (and since its a google spreadsheet, multiple people can update/revise the list) and all the programmer needs to do is write ERB to iterate through your list like you would in a rails app. To ensure that your local data matches the data that is being inputted onto the google spreadsheet, you can run the rake task before deployment to get the most accurate, up to date data list without having to manually input the data in the HTML.

Check out the code below (or the github link to my [sample repo](https://github.com/landonmarder/google_drive_ruby)) for the Ruby of getting your spreadsheet from Google into json and a commented out sample of what the ERB in your Middleman app will look like to get the data from the json.

{% highlight ruby %}
require "rubygems"
require "google_drive"
require 'json'

# Password is located in a .secret file
password = File.open(".secret", "r").read
password.gsub!("\n", '')

# Authenticate with Google
session = GoogleDrive.login("your-email@gmail.com", password)

# Get the spreadsheet you want
# Spreadsheet URL: https://docs.google.com/spreadsheet/ccc?key=0AornRwZY3X2FdDdpRUhnMG1wMWJOdEtGTGZDbmNTYkE&usp=drive_web#gid=0

google_spreadsheet_key = '0AornRwZY3X2FdDdpRUhnMG1wMWJOdEtGTGZDbmNTYkE&usp'
rows = session.spreadsheet_by_key(google_spreadsheet_key).worksheets[0].rows

companies = Array.new

rows[1..-1].each do |row|
  companies << { name: row[0], url: row[1], category: row[2] }
end


# Write array into the file as json
File.open('data/companies.json', 'w') { |f| f << companies.to_json }

# How to access the json file in middleman (file located in data/companies.json in your middleman app)
# <% data.companies.each do |company| %>
#    <%= company[:name]%>
# <% end %>

end

{% endhighlight %}
