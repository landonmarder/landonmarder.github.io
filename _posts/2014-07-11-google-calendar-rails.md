---
layout: post
title: CRUD Actions with Google Calendar and Rails
categories: posts
date: 2014-07-11 11:00:00
summary: I am going to go through the complete steps of adding, updating, and deleting a event from an Event Dashboard in your Rails app that will also add, update, and delete that event in a corresponding Google Calendar (I am adding, updating, and deleting events to my personal calendar, but you can change the calendarId option in the parameters to any calendar that you have admin access to).
---

In a [previous post](http://landonmarder.com/posts/2014/06/04/google-cal-rails/), I talked about setting up Google's OAuth with a Rails app. Today I am going to go through the complete steps of adding, updating, and deleting a event from an Event Dashboard in your Rails app that will also add, update, and delete that event in a corresponding Google Calendar (I am adding, updating, and deleting events to my personal calendar, but you can change the calendarId option in the parameters to any calendar that you have admin access to).

Step 1: Getting your Event Model Ready for Google Calendar
----
The first thing we need to do is to add another attribute to our events model so that we can store the google event id after we create the event and push it to our Google Calendar. For simplicity, I called this google_event_id and gave it a string attribute.

Step 2: Creating an Event
----
We need to update the create action in our events controller so that after we make the API call to add the event to the Google Calendar, we save the google event id to the event object. We will need this google event id in the future to update and delete google calendar events from our Rails app.

{% highlight ruby %}
def create
    @event = Event.new(event_params)

    if @event.save

      render :nothing => true
      google_event = { 'summary' => @event.title, 'description' => @event.description, 'location' => @event.location, 'start' => {'dateTime' => @event.start_time}, 'end' => {'dateTime' => @event.end_time}, 'attendees' => [{"email" =>'landon.marder@gmail.com'}] }

      client = Google::APIClient.new
      client.authorization.access_token = current_user.token
      service = client.discovered_api('calendar', 'v3')

      result = client.execute(:api_method => service.events.insert,
                              :parameters => {'calendarId' => 'landon.marder@gmail.com', 'sendNotifications' => true},
                              :body => JSON.dump(google_event),
                              :headers => {'Content-Type' => 'application/json'})

      @event.google_event_id = result.data.id
      @event.save
    else
      render :nothing => true
    end
  end
{% endhighlight %}

Step 3: Updating an Event
----
Now that we have a google_event_id as an attribute of each newly created event model, we can query the Google Calendar API for that specific event and update it in our events controller. I gave a conditional in case there is no google_event_id (for historical events that were not tied to the google calendar)

{% highlight ruby %}
def update
  @event = Event.find(params[:id])


  if @event.update(event_params)
    render :nothing => true

    unless @event.google_event_id.nil?
      client = Google::APIClient.new
      client.authorization.access_token = current_user.token
      service = client.discovered_api('calendar', 'v3')

      result = client.execute(:api_method => service.events.get, :parameters => {'calendarId' => 'landon.marder@gmail.com', 'eventId' => @event.google_event_id } )

      event = result.data
      event.summary = @event.title
      event.start.dateTime = @event.start_time
      event.end.dateTime = @event.end_time
      event.description = @event.description
      event.location = @event.location

      result = client.execute(:api_method => service.events.update,
                              :parameters => {'calendarId' => 'landon.marder@gmail.com', 'eventId' =>  @event.google_event_id},
                              :body_object => event,
                              :headers => {'Content-Type' => 'application/json'})
    end
  else
    render :nothing => true
  end
end
{% endhighlight %}

Step 4: Deleting an Event
----
Similarly, with the google_event_id attribute of the event object, you can delete an event from the events controller.
{% highlight ruby %}
def destroy
  @event = Event.find(params[:id])

  @event.destroy

  if @event.destroyed?
    unless @event.google_event_id.nil?
      client = Google::APIClient.new
      client.authorization.access_token = current_user.token
      service = client.discovered_api('calendar', 'v3')

      result = client.execute(:api_method => service.events.delete,
                              :parameters => {'calendarId' => 'landon@work-bench.com', 'eventId' => @event.google_event_id})
    end
  end
  redirect_to events_path, notice: "#{@event.title} - has been removed."
end
{% endhighlight %}
