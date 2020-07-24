---
layout: post
title: First steps on Elixir and Phoenix
categories: blog
intro: 
---

## Good services to watch insde Realm
 - nation-broker
> example of scheduler, translator and sending messages.
 - notification-service.

**Live reloading** is built-in
Phoenix uses node.js
Phoenix uses **Brunch** to build js and css assets.

### scheme `scheme/2`
Scheme is the equivalent to an SQL Table, is a macro defined in EctoScheme.

use NationBorker: 

### Import macro:
The `use` macro is defined with __using__ 
 - this is a good way to organize commum imports.
the `import` macro, 
the `alias` macro, 

`@` keyword defines a constant macro

nation_broker_msg is good place to see the defines contants.
On line 11 it returns the :message_bus and assign it to @message_bus


### Quote and unquote
Serach about that, seams really cool. Spoiler: This is a way to declare differnt implementation depending on the module.

### Ecto
To create a new table:
➜  user-service git:(CG-159_bold_calc_costs_updates) mix ecto.gen.migration create_push_token_import_records_table

Ecto changeset 
example:
defmodule NotificationService.PushToken do
    # This just make sure the entrance is valid, and not expose db to invalid entries, and avoid IO usage.
    def changeset(struct, params \\ %{}) do # We need this changeset to insert things in the db, for this we need sonme transofrmations. All provided by Ecto, our little friend
    struct
    |> cast(params, @fields) # Make casts to adjust types, and check if anything breaks.
    |> validate_required(@required_fields) # Check no null canstraints.
    |> unique_constraint(:user_id) # Check unique_id constraint.
    |> foreign_key_constraint(:user_id) # Check for 
    end
end




```elixir
# This is a schema
defmodule NotificationService.PushNotification.PushToken do # This just make sure the entrance is valid, and not expose db to invalid entries, and avoid IO usage.
  use NotificationService, :model 
  
   # We need this changeset to insert things in the db, for this we need sonme transofrmations. All provided by Ecto, our little friend
  def changeset(struct, params \\ %{}) do
    struct
    |> cast(params, @fields) # Make casts to adjust types, and check if anything breaks.
    |> validate_required(@required_fields) # Check no null canstraints.
    |> unique_constraint(:user_id) # Check unique_id constraint.
    |> foreign_key_constraint(:user_id) # Check for 
  end
end
```

``` elixir
defmodule NotificationServiceMsg.PushTokenConsumer do # This is a Consumer
  use NotificationServiceMsg, :consumer
  require Logger

  def process(%{"data" => data, "action" => action})
  when action in ["created"],
    do: { :ok, _token } = PushNotification.insert_push_token(data) #This is a shortcut to patternmatching, in case of errors this will break in the `dead letter queue`

  def process(_data), do: :ok
end
```

```
defmodule NotificationService.PushNotification do # This is a Context.
  alias NotificationService.Repo
  alias NotificationService.PushNotification.PushToken

  def insert_push_token(push_token_params) do
    %PushToken{
      id: Ecto.UUID.generate()# This add a uuid for the to_insert push token.
    }
    |> changeset(push_token_params)
    |> Repo.insert()
  end
end

```


### Poison
This an example of encoder to implement inside a model class:
`@derive {Poison.Encoder, only: [:user_id, :token, :created_at]}`

### Recieve messages
- Create a comsumer

### Send messages
- Create a producer

- it uses a dependency, called [kafka_message_bus](https://github.com/jeffhsta/kafka_message_bus/)

- this is an [important file](https://github.com/jeffhsta/kafka_message_bus/blob/master/lib/kafka_message_bus.ex)

```elixir 
defmodule NationBrokerMsg.Realm.UsersPushTokenProducer do
  use NationBrokerMsg, :producer
  alias NationBroker.Realm.PushToken

  @resource "push_token"
  @topic "notification" 
  @action "created"
  def push_token_created(push_token = %PushToken{}) do
    @message_bus.produce(
      push_token,
      push_token.user_id,
      @resource,
      @action,
      source: "nation",
      topic: @topic
    )
  end
end
```

### A context in Phoenix
Is a parent folder that descirbes a bunch of modules.
studies resource: https://hexdocs.pm/phoenix/contexts.html
All things related to this context, shoulkd be here. i.e: CRUD, and filters, blablabla


### Composed query with Ecto

This is really good docs: https://hexdocs.pm/ecto/Ecto.Query.html

def that uses query as the first parameter i.e: `query_all/2`

### module
To create a module:
    Elixir files are modules.
    The last word o `defmodule` by convention describes the file. 
    i.e:
    PushTokenImportRecord is push_token_import_record.


# Docker 
## Troubleshooting docker

For most errors on elixir, mainly related to connection, is due to Docker, som helpfull commands are bellow to help you

Remove all images:
```
docker-compose down # Down dockers
docker rm $(docker ps -aq) # Remove all process
docker rmi $(docker images -aq) # Remove all images
docker-compose up -d # Up docker in background
```

studie - Dependecy injection in Elixir


Resources:
https://app.pluralsight.com/library/courses/elixir-getting-started
https://app.pluralsight.com/library/courses/phoenix-getting-started

https://elixir-examples.github.io/examples/phoenix-framework-from-http-request-to-response

CheatSheet
https://devhints.io/elixir

https://devhints.io/phoenix-ecto

https://learnxinyminutes.com/docs/elixir/


REASTFull Api very easy to implement simple things:
https://github.com/elixir-maru/maru


Also I should add a link checker to the blog :)

https://github.com/BohdanOrlov/iOS-Developer-Roadmap

https://medium.com/@likid.geimfari/the-list-of-interesting-open-source-projects-2daaa2153f7c