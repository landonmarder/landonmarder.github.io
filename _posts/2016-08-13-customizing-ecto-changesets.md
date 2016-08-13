---
layout: post
title: Customizing Ecto Changesets to Validate Data and Ensure Data Integrity
categories: posts
date: 2016-08-13 11:15:00
summary: Ecto Changesets are a powerful way to validate data before inserting it into the database
---

Ecto changesets help us cast, validate, filter, and manipulate data.
This post will walk through how to leverage Ecto changesets to validate data
before we insert it into the database to ensure data integrity.
For example, let's say that in our app, we can create a user with a name,
but we want to validate the name before we insert it into the database.

Before diving into what Ecto changesets can do to validate our data,
let's first look at a barebones create action in a Phoenix controller and see how
changesets play a role in our controller. Since we only expect the user to enter a name,
the `user_params` will look like `{name: "Michael Jordan"}`.

{% highlight elixir %}
def create(conn, %{"user" => user_params}) do
  changeset = User.changeset(%User{}, user_params)

  case Repo.insert(changeset, conn: conn) do
      {:ok, user} ->
        conn
        |> put_status(:created)
        ...
      {:error, changeset} ->
        conn
        |> put_status(:unprocessable_entity)
        ...
    end
{% endhighlight %}

At a very high level, the code above passes the `user_params` into the changeset and
then tries to insert the changeset into the database. Depending on the changeset,
the insert will either be successful with a  `{:ok, _}` and we return a `200 status`
or the insert will not be successful `{:error, _}` and we return a `422 status`.

Out of the box, the changeset function will simply cast the data to prepare it to be inserted into
the database. This is what a simple changeset function inside the User model looks like:

{% highlight elixir %}
def changeset(model, params \\ :empty) do
  model
  |> cast(params, @required_fields, @optional_fields)
end
{% endhighlight %}

We pass in the `%User{}` as our `model` and `user_params` as our `params`.
We cast the `name` field and the changeset function will return:

{% highlight elixir %}
%Ecto.Changeset{action: nil, changes: %{name: "Michael Jordan"},
constraints: [], errors: [], filters: %{} ...}
{% endhighlight %}

Note how the `constraints` and `errors` are empty! This means the changeset is saying
the changes are. We will have to add code to our changeset
so that if the changes are not valid, the changeset will populate the `constraints` or `errors`.

**Adding A Built-In Constraint**

Let's say that we want to add a constraint that the name needs to be unique. We cannot
have two users with the same name because that will be confusing. Luckily, Ecto has
[built in constraint](https://hexdocs.pm/ecto/Ecto.Changeset.html)
that we can take advantage of.

Below, we are adding a unique constraint on the name field into the changeset:

{% highlight elixir %}
def changeset(model, params \\ :empty) do
  model
  |> cast(params, @required_fields, @optional_fields)
  |> unique_constraint(:name)
end
{% endhighlight %}

And if we now try to insert a new user with the name `Michael Jordan`, the changeset
will look like this:

{% highlight elixir %}
%Ecto.Changeset{action: nil, changes: %{name: "some content"},
 constraints: [%{constraint: "users_name_index", field: :name,
    message: "has already been taken", type: :unique}], .. }
{% endhighlight %}

Aha! The changeset now includes `constraints: [%{constraint: "users_name_index", field: :name,
message: "has already been taken", type: :unique}`. Now, when we do `Repo.insert(changeset, conn: conn)`,
it will not insert the data and instead will return `{:error, _}`.

**Adding a Custom Constraint**

We can also build our own validations if the ones that Ecto provides out of the box
are insufficient. For example, let's say that we want to validate the uniqueness of
all names, but for some reason, we do not want to validate the uniquess of name if the name is `Michael Jordan`.
We are okay with as many users as possible with the name `Michael Jordan`, but only `Michael Jordan`
(I know, a silly example :smile:).

{% highlight elixir %}
def changeset(model, params \\ :empty) do
  model
  |> cast(params, @required_fields, @optional_fields)
  |> special_unique_name_validation
end

defp special_unique_name_validation(changeset) do
  new_name = get_field(changeset, :name)

  cond do
    new_name == "Michael Jordan" ->
      changeset
    true ->
      changeset
      |> unique_constraint(:name)
  end
end
{% endhighlight %}

In the above example, we include a new private function `special_validate_unique_name`
in the changeset and pass the changeset into it. Inside the function, we get the name from the changeset
and see if the name is `Michael Jordan`. If the name is `Michael Jordan`, then we do
nothing and pass the changeset through. If the name is not `Michael Jordan`, we take
the changeset and add a unique constraint to it for the name.

{% highlight elixir %}
%Ecto.Changeset{action: nil, changes: %{name: "Lebron James"}, constraints: [],
 errors: [name: "has already been taken."]
{% endhighlight %}

It works!

**Adding a Validation**

We have only been looking at constraints, but we can also add built in and custom
validations to our changeset. For example, let's say that we also want to make sure
that our name field does not go above 20 characters. We can add Ecto's built in
`validate_length` validation.

{% highlight elixir %}
def changeset(model, params \\ :empty) do
  model
  |> cast(params, @required_fields, @optional_fields)
  |> validate_length(:name, max: 20)
  |> special_unique_name_validation
end
{% endhighlight %}

Trying to insert a user with a name longer than 20 characters with the changeset function
will yield the following changeset:

{% highlight elixir %}
%Ecto.Changeset{action: nil, changes: %{name: "This is a super long name"}, constraints: [],
 errors: [name: {"should be at most %{count} character(s)", [count: 20]}] .. }
{% endhighlight %}

Just like when the changesets includes `constraints`, when the changeset includes
`errors` and we do `Repo.insert(changeset, conn: conn)`, it will not insert the data and instead will return `{:error, _}`.
