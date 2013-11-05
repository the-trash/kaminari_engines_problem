## Kaminari + Engines Problem

### How to Install and start test App

```
git clone https://github.com/the-teacher/kaminari_engines_problem.git

cd kaminari_engines_problem

bundle

rake db:bootstrap_and_seed

rails s
```

### Browser

Visit 

```ruby
http://localhost:3000/
```

And page with problem

```ruby
http://localhost:3000/comments/manage
```

### Description

gem TheComments mount routes with

**config/routes.rb**

```ruby
App::Application.routes.draw do
  root 'posts#index'
  resources :posts

  # ..

  mount TheComments::Engine => '/', as: :comments
end
```

kaminari should paginate Admin UI for incoming comments. But it doesn't work

**app/views/the_comments/haml/manage.html.haml**

```ruby
= paginate @comments
```

Only with Patch **config/initializers/kaminari_route_proxy.rb** it works fine

```ruby
module Kaminari
  module Helpers
    class Tag
      def page_url_for(page)
        (@options[:routes_proxy] || @template).url_for @params.merge(@param_name => (page <= 1 ? nil : page))
      end
    end
  end
end
```

**app/views/the_comments/haml/manage.html.haml**

```ruby
= paginate @comments, routes_proxy: comments
```