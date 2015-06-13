---
layout: post
title: Adding an Event to Google Calendar from a Rails App
categories: posts
date: 2014-06-04 11:00:00
summary: I needed to create an Events API that could talk to multiple apps. In addition, it was preferable that the Events API play well with Google Calendar. To solve this problem, I integrated OmniAuth with Google into my current Devise setup and then, used the Google Calendar API to interact with my Rails App.
---

I needed to create an Events API that could talk to multiple apps. In addition, it was preferable that the Events API play well with Google Calendar. To solve this problem, I integrated OmniAuth with Google into my current Devise setup and then, used the Google Calendar API to interact with my Rails App. I used the following three resources to help me complete this task [Greg Baugues](http://blog.baugues.com/google-calendar-api-oauth2-and-ruby-on-rails), [Deepak Dash](http://deepakrip007.wordpress.com/2013/11/05/google-integration-using-devise-and-omniauth-in-rails-app/), and [the Devise Docs](https://github.com/plataformatec/devise/wiki/OmniAuth:-Overview), but I want to explain the steps to better comprehend everything that I did and hopefully help others connect Rails and Google Calendar in the future.

Step 1:
---
You want to get the necessary gems (omniauth and google-api-client). The omniauth gems are used to authenticate with Google so that you can interact with its API. Google-api client is for the actual interacting with Google's API.

{% highlight ruby %}
gem 'google-api-client', :require => 'google/api_client'
gem 'omniauth'
gem 'omniauth-google-oauth2'
{% endhighlight %}

Step 2:
---
You need to create a project on [Google's developer console](https://console.developers.google.com/project). Turn on the Calendar API (what you want your Rails app to interact with) and the Google+ API (needed for authenticating to allow you to access the Calendar API). You can turn off the other APIs that Google automatically turns on because you are not using them. Then, create a new client ID for a web application. By doing so, you will get a Client ID and a Client Secret ID that you need to put in your .env file of your Rails app. For local development, you also at this time want to give a redirect URI of:

{% highlight html %}
http://localhost:3000/users/auth/google_oauth2/callback
{% endhighlight %}

Step 3:
---
Step 3 assumes that you already have devise set up under a model User. If you do not have Devise set up yet, do so first. The main idea of this step is that you want to tell Devise that you are going to be 'omniauthable' and what you should do once you have authenticated with Google.

{% highlight ruby %}
# config/initializers/devise.rb
require 'omniauth-google-oauth2'
  config.omniauth :google_oauth2, ENV['GOOGLE_CLIENT_ID'],
  ENV['GOOGLE_CLIENT_SECRET'],
  { access_type: "offline", approval_prompt: "",
  scope: 'userinfo.email,calendar' }
{% endhighlight %}

This tells the Google API that you have the appropriate id and secret password to access the Google API and the scope says which of Google's APIs you want to use.

{% highlight ruby %}
# app/models/user.rb
devise :database_authenticatable, :registerable,
       :recoverable, :rememberable, :trackable, :validatable,
       :omniauthable, :omniauth_providers => [:google_oauth2]
{% endhighlight %}

This alerts devise that you want to use omniauth and specifically use google as your omniauth provider

{% highlight ruby %}
# config/routes.rb
devise_for :users,
:controllers => { :omniauth_callbacks => "omniauth_callbacks" }
{% endhighlight %}
This tells your router which controller action to go to when your user clicks on the link to the user_omniauth_authorize_path

{% highlight ruby %}
# app/controllers/omniauth_callbacks_controller.rb
class OmniauthCallbacksController < Devise::OmniauthCallbacksController
  def google_oauth2
    @user = User.find_for_google_oauth2(request.env["omniauth.auth"], current_user)

    if @user
      flash[:notice] = I18n.t "devise.omniauth_callbacks.success", :kind => "Google"
      sign_in_and_redirect @user, :event => :authentication
    else
      redirect_to new_user_session_path, notice: 'Access Denied'
    end
  end
end
{% endhighlight %}

This creates the controller to handle the router action when you get the callback from Google telling you that you authenticated successfully. The method for this will be written in step 5 with the idea being that I want to restrict who can access the site.

Step 4:
---
Run a migration to add the following to your User table: token (string), uid (string), provider (string). We will need these pieces of information from authenticating with Google in the future to interact with the Google Calendar API.

Step 5:
---
Create a method in your User class that finds the user by their email. Then, save Google authentication characteristics to that user so you can persist that data throughout the user's session. The idea is that you need a user's token to interact successfully with the Google Calendar API. By assigning that token to the user, you will be able to access the token. Every time the user signs in, it updates his/her token. Tokens expire in an hour.

{% highlight ruby %}
def self.find_for_google_oauth2(access_token, signed_in_resource=nil)
  data = access_token.info
  if (User.admins.include?(data.email))
    user = User.find_by(email: data.email)
    if user
      user.provider = access_token.provider
      user.uid = access_token.uid
      user.token = access_token.credentials.token
      user.save
      user
    else
      redirect_to new_user_registration_path, notice: "Error."
    end
  end
end
{% endhighlight %}

Note, in my app, all the users first need to register without Google omniauth because I want to restrict who can sign up. Also, there is a select group of users who will only be given an access code and this will correlate with an account that has more restrictions about what they can do. These users will share a User account that is not tied to Google.

Step 6:
---
At this point, you are good to go with interacting with the Google Calendar API! To demonstrate the power of what I did above, I created a stub event that writes to my personal calendar.

{% highlight ruby %}
@event = {
  'summary' => 'New Event Title',
  'description' => 'The description',
  'location' => 'Location',
  'start' => { 'dateTime' => Chronic.parse('tomorrow 4 pm') },
  'end' => { 'dateTime' => Chronic.parse('tomorrow 5pm') },
  'attendees' => [ { "email" => 'bob@example.com' },
  { "email" =>'sally@example.com' } ] }

client = Google::APIClient.new
client.authorization.access_token = current_user.token
service = client.discovered_api('calendar', 'v3')

@set_event = client.execute(:api_method => service.events.insert,
                        :parameters => {'calendarId' => current_user.email, 'sendNotifications' => true},
                        :body => JSON.dump(@event),
                        :headers => {'Content-Type' => 'application/json'})
{% endhighlight %}

The client sets up a new connection to Google's API. You tell the API that you are authorized to use it using your user's token. Since you've turned on the calendar API on Google's side in step 2 and scoped for the calendar API in your app in step 3, you are authorized to discover the calendar API. The event above has a title, description, location, start time, end time, and gets sent to two individuals. Moreover, I set the sendNotifications parameter to true so that every attendee will get an email when I create the event. Check out the [Google Calendar API documentation on inserting event](https://developers.google.com/google-apps/calendar/v3/reference/events/insert) for more parameters and requests.

To tie this to my Events API, in the future, a successful creation of an Events entry will toggle the app to send an API request to create a Google Calendar request so that a Google Calendar Event gets created every time an Event from my Rails API gets created. They will be in two places, but will only need to input the Event data once.
