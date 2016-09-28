# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  Join tables are valuable because they allow you to have a different view of
  your data while preserving the original state of your data. They allow you to
  make connections and link data tables that have relationships.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
create_table "profiles", force: :cascade do |t|
    t.string  "given_name"
    t.string "surname"
    t.string  "email"
    t.integer  "profile_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "movies", force: :cascade do |t|
      t.string  "title"
      t.datetime "release_date"
      t.integer  "length"
      t.integer  "movie_id"
      t.datetime "created_at", null: false
      t.datetime "updated_at", null: false
    end

    create_table "favorites", force: :cascade do |t|
        t.string  "title"
        t.integer  "movie_id"
        t.integer  "profile_id"
        t.datetime "created_at", null: false
        t.datetime "updated_at", null: false
      end

add_index "movies", ["movie_id"], name: "index_favorites_on_movie_id", using: :btree
add_index "profiles", ["profile_id"], name: "index_favorites_on_profile_id", using: :btree
```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
has_many :movies, through: :favorites
has_many :favorites
end
```

```rb
class Movie < ActiveRecord::Base
has_many :profiles, through: :favorites
has_many :favorites
end
```

```rb
class Favorite < ActiveRecord::Base
belongs_to :doctor, inverse_of: :favorites
belongs_to :patient, inverse_of: :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
The serializer creates JSON messages that helps communicate with your server.
```

```rb
class ProfileSerializer < ActiveModel::Serializer
attributes :profile_id, :given_name, :surname, :email, :movies, :favorites
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
bundle exec rails g scaffold Favorites title:string movie:references profile:references
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
  It is used when we want to delete something on a join table but dont want to lose
  the information that it is associated with on other tables.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
  # < Your Response Here >
```
