# Data Model Task

From The Odin Project's [curriculum](https://www.theodinproject.com/courses/ruby-on-rails/lessons/building-with-active-record-ruby-on-rails)

## Online learning platform

You are building an online learning platform (much like this!). You’ve got many different courses, each with a title and description, and each course has multiple lessons. Lesson content consists of a title and body text.

Courses
```ruby
title:string [unique, x chars, present]
description:text [present]
id:integer

has_many :lessons
```

Lessons
```ruby
title:string [unique, present]
body:text [present]

id:integer
course_id:integer [present]

belongs_to :course
```

## User profile

You are building the profile tab for a new user on your site. You are already storing your user’s username and email, but now you want to collect demographic information like city, state, country, age and gender. Think – how many profiles should a user have? How would you relate this to the User model?

Users
```ruby
username:string [unique, x chars, present]
email:string [unique, present, regex]

id:integer
age_id:integer [present]
gender_id:integer [present]
city_id:integer [present]
state_id:integer [present]
country_id:integer [present]

belongs_to :age
belongs_to :gender
belongs_to :city
belongs_to :state
belongs_to :country
```
Ages
```ruby
years:integer [min 0, max 120]

id:integer

has_many :users
```
Genders
```ruby
name:string [one char, either M or F]

id:integer

has_many :users
```
Countries
```ruby
name:string [unique, present]

id:integer

has_many :users
has_many :states
has_many :cities, through: :states
```
States
```ruby
name:string [present]

id:integer
country_id:integer [present]

has_many :users
belongs_to :country
has_many :cities
```
Cities
```ruby
name:string [present]

id:integer
state_id:integer [present]

has_many :users
belongs_to :state
```

## Virtual pinboard

You want to build a virtual pinboard, so you’ll have users on your platform who can create “pins”. Each pin will contain the URL to an image on the web. Users can comment on pins (but can’t comment on comments).

Users
```ruby
username:string [unique, x chars, present]
password:string [x chars, present]

id:integer

has_many :pins
has_many :comments
```
Pins
```ruby
url:string [regex, present]

id:integer
user_id:integer [present]

belongs_to :user
has_many :comments
```
Comments
```ruby
title:string [present, x chars]
body:text [present, x chars]

id:integer
user_id:integer [present]
pin_id:integer [present]

belongs_to :user
belongs_to :pin
```

## Hacker news

You want to build a message board like [Hacker News](http://news.ycombinator.com/). Users can post links. Other users can comment on these submissions or comment on the comments. How would you make sure a comment knows where in the hierarchy it lives?

Users
```ruby
username:string [unique, x chars, present]
password:string [x chars, present]

id:integer

has_many :links
has_many :comments
```
Links
```ruby
url:string

id:integer
user_id:integer [present]

belongs_to :user
has_many :comments
```
Comments
```ruby
title:string [present, x chars]
body:text [present, x chars]

id:integer
user_id:integer [present]
link_id:integer [present]
parent_comment_id:integer
child_comment_id:integer [present]

belongs_to :user
belongs_to :link
belongs_to :parent, class_name: "Comment", foreign_key: "parent_comment_id", optional: true
has_many :child_comments, class_name: "Comment", foreign_key: "child_comment_id"
```
