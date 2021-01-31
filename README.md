# Ruby-on-Rails Guide Notes

Completing the Rails Guide at https://guides.rubyonrails.org/getting_started.html

## Terminology

**Controller:** A controller's purpose is to receive specific requests for the application. Routing decides which controller receives which requests. Often, there is more than one route to each controller, and different routes can be served by different actions. 

**Action:** Each action's purpose is to collect information to provide it to a view.

**View:** A view's purpose is to display this information in a human readable format. An important distinction to make is that it is the controller, not the view, where information is collected. The view should just display that information. 

**eRuby:** Short for "Embedded Ruby", file format `.erb`. Designed to embed Ruby in HTML. Default language for view templates. Processed by the request cycle in Rails before being sent to the user.

**Routing File:** `config/routes.rb` - holds entries in a special DSL that tells Rails how to connect incoming requests to controllers and actions.

**Format:** _Views_ - First extension in a view template name, ex: `new.html.erb` where `html` is the format. Specifies content type of the view.

**Handler:** _Views_ - Second extension in a view template name, ex: `new.html.erb` where `erb` is the handler. Specifies how the template will be rendered.

## Commands

`rails new blog`: Create a Rails application called Blog in a blog directory and install the gem dependencies that are already mentioned in Gemfile using bundle install.

`rails server`: Fire up Puma, a web server distributed with Rails by default. To see your application in action, open a browser window and navigate to http://localhost:3000.

`rails generate controller <name> <action>`: Run "controller" generator to create controller called "name" with "action".

`rails routes`: List defined routes for all the standard RESTful actions. 

`rails generate model <name> <field>:<type> <record>:references`: Create a model class, and corresponding DB migration.

`rails db:migrate`: Triggers Rails to execute the migration command.

`rails db:rollback`: Undo execution of migrate

`rails db:migrate VERSION=0`: Specify schema version to revert to

`rails generate controller StaticPages home help`: Create StaticPages controller with home and help methods

`rails destroy controller StaticPages home help`: Destroy StaticPages controller and methods

## Rails App Directory Structure

|File / Folder   |Purpose   |
|---|---|
|`app/`   |Contains the controllers, models, views, helpers, mailers, channels, jobs, and assets for your application. You'll focus on this folder for the remainder of this guide.   |
|`bin/`   |Contains the rails script that starts your app and can contain other scripts you use to setup, update, deploy, or run your application.   |
|`config/`   |Configure your application's routes, database, and more. This is covered in more detail in [Configuring Rails Applications](https://guides.rubyonrails.org/configuring.html).   |
|`config.ru`   |Rack configuration for Rack based servers used to start the application. For more information about Rack, see the [Rack website](https://rack.github.io/).   |
|`db/`   |Contains your current database schema, as well as the database migrations.   |
|`Gemfile Gemfile.lock`   |These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem. For more information about Bundler, see the [Bundler website](https://bundler.io/).   |
|`lib/`   |Extended modules for your application.   |
|`log/`   |Application log files.   |
|`package.json`   |This file allows you to specify what npm dependencies are needed for your Rails application. This file is used by Yarn. For more information about Yarn, see the [Yarn website](https://yarnpkg.com/lang/en/).   |
|`public`   |The only folder seen by the world as-is. Contains static files and compiled assets.   |
|`Rakefile`   |This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Rails. Rather than changing `Rakefile`, you should add your own tasks by adding files to the `lib/tasks` directory of your application.   |
|`Rakefile`   |This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Rails. Rather than changing `Rakefile`, you should add your own tasks by adding files to the `lib/tasks` directory of your application.   |
|`README.md`   |This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on.   |
|`storage/`   |Active Storage files for Disk Service. This is covered in [Active Storage Overview](https://guides.rubyonrails.org/active_storage_overview.html).   |
|`test/`   |Unit tests, fixtures, and other test apparatus. These are covered in Testing Rails [Applications](https://guides.rubyonrails.org/testing.html).   |
|`tmp/`   |Temporary files (like cache and pid files).   |
|`vendor`   |A place for all third-party code. In a typical Rails application this includes vendored gems.   |
|`.gitignore`   |This file tells git which files (or patterns) it should ignore. See GitHub - Ignoring files for more info about ignoring files.   |
|`.ruby-version`|This file contains the default Ruby version.  |

## REST Mappings
|HTTP Method | Path | Class Method | Description
|GET |	/users |	index |	page to list all users|
|GET |	/users/1 |	show |	page to show user with id 1|
|GET |	/users/new |	new |	page to make a new user|
|POST |	/users 	create |	|create a new user|
|GET |	/users/1/edit |	edit |	page to edit user with id 1|
|PATCH |	/users/1 |	update |	update user with id 1|
|DELETE| 	/users/1 |	destroy |	delete user with id 1|


## Notes

Rails comes with a number of scripts called generators that are designed to make your development life easier by creating everything that's necessary to start working on a particular task. One of these is the new application generator, which will provide you with the foundation of a fresh Rails application so that you don't have to write it yourself.

`rails generate controller Welcome index` creates the following files and route:

```
create  app/controllers/welcome_controller.rb
 route  get 'welcome/index'
invoke  erb
create    app/views/welcome
create    app/views/welcome/index.html.erb
invoke  test_unit
create    test/controllers/welcome_controller_test.rb
invoke  helper
create    app/helpers/welcome_helper.rb
invoke    test_unit
invoke  assets
invoke    scss
create      app/assets/stylesheets/welcome.scss
```

Rails provides a `resources` method which can be used to declare a standard REST resource. You need to add the article resource to the config/routes.rb so the file will look as follows:

```
Rails.application.routes.draw do
  get 'welcome/index'
 
  resources :articles
 
  root 'welcome#index'
end
```

A controller is simply a class that is defined to inherit from ApplicationController. It's inside this class that you'll define methods that will become the actions for this controller. These actions will perform CRUD operations on the articles within our system.

To create a form within a template, you will use a form builder. The primary form builder for Rails is provided by a helper method called `form_with`.

```
<%= form_with scope: :article, local: true do |form| %>
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
<% end %>
```

Models in Rails use a singular name, and their corresponding database tables use a plural name. Rails provides a generator for creating models, which most Rails developers tend to use when creating new models. To create the new model, run this command in your terminal:
`$ rails generate model Article title:string text:text`

Because you're working in the development environment by default, `rails db:migrate` will apply to the database defined in the development section of your config/database.yml file (`db/development.sqlite3`). 
If you would like to execute migrations in another environment, for instance in production, you must explicitly pass it when invoking the command: rails db:migrate RAILS_ENV=production.

Rails has several security features that help you write secure applications, and you're running into one of them now. This one is called strong parameters, which requires us to tell Rails exactly which parameters are allowed into our controller actions. his is often 
factored out into its own method so it can be reused by multiple actions in the same controller, for example create and update. Above and beyond mass assignment issues, the method is often made private to make sure it can't be called outside its intended context.

A frequent practice is to place the standard CRUD actions in each controller in the following order: index, show, new, edit, create, update and destroy. You may use any order you choose, but keep in mind that these are public methods; as mentioned earlier in this guide, 
they must be placed before declaring private visibility in the controller.

The `link_to` method is one of Rails' built-in view helpers. It creates a hyperlink based on text to display and where to go - in this case, to the path for articles.

`<%= link_to 'My Blog', controller: 'articles' %>`

If you want to link to an action in the same controller, you don't need to specify the `:controller` option, as Rails will use the current controller by default.

`ApplicationRecord` inherits from `ActiveRecord::Base` which supplies a great deal of functionality to your Rails models for free, including basic database CRUD (Create, Read, Update, Destroy) operations, data validation, as well as sophisticated search support and the ability to relate multiple models to one another.

Rails can validate a variety of conditions in a model, including the presence or uniqueness of columns, their format, and the existence of associated objects. Validations are covered in detail in [Active Record Validations](https://guides.rubyonrails.org/active_record_validations.html).

Notice that inside the `create` action we use `render` instead of `redirect_to` when save returns false. The `render` method is used so that the `@article` object is passed back to the `new` template when it is rendered. This rendering is done within the same request as the form submission, whereas the `redirect_to` will 
tell the browser to issue another request.

`pluralize` is a rails helper that takes a number and a string as its arguments. If the number is greater than one, the string will be automatically pluralized.

Rails automatically wraps fields that contain an error with a div with class field_with_errors. You can define a CSS rule to make them standout.

Passing the article object to the `form_with` method will automatically set the URL for submitting the edited article form. This option tells Rails that we want this form to be submitted via the `PATCH` HTTP method, which is the HTTP method you're expected to use to update resources according to the REST protocol.

Also, passing a model object to `form_with`, like `model: @article` in the edit view above, will cause form helpers to fill in form fields with the corresponding values of the object. Passing in a symbol scope such as `scope: :article`, as was done in the new view, only creates empty form fields. More details can be found 
in [form_with documentation](https://api.rubyonrails.org/v6.0.3.2/classes/ActionView/Helpers/FormHelper.html#method-i-form_with).

It is not necessary to pass all the attributes to `update`. For example, if `@article.update(title: 'A new title')` was called, Rails would only update the `title` attribute, leaving all other attributes untouched.

By convention, partial files are prefixed with an underscore. You can read more about partials in the [Layouts and Rendering in Rails](https://guides.rubyonrails.org/layouts_and_rendering.html) guide.

 The reason we can use this shorter, simpler form_with declaration to stand in for either of the other forms is that @article is a resource corresponding to a full set of RESTful routes, and Rails is able to infer which URI and method to use. For more information about this use of `form_with`, 
 see [Resource-oriented style](https://api.rubyonrails.org/v6.0.3.2/classes/ActionView/Helpers/FormHelper.html#method-i-form_with-label-Resource-oriented+style).
 
 Learn more about Unobtrusive JavaScript on [Working With JavaScript in Rails](https://guides.rubyonrails.org/working_with_javascript_in_rails.html) guide.
 
 In general, Rails encourages using resources objects instead of declaring routes manually. For more information about routing, see [Rails Routing from the Outside In](https://guides.rubyonrails.org/routing.html).
 
 The (`:references`) keyword used in the bash command is a special data type for models. It creates a new column on your database table with the provided model name appended with an `_id` that can hold integer values.
 
 For more information on Active Record associations, see the [Active Record Associations](https://guides.rubyonrails.org/association_basics.html) guide.
 
 For more information on routing, see the [Rails Routing](https://guides.rubyonrails.org/routing.html) guide.
 
 Other authentication methods are available for Rails applications. Two popular authentication add-ons for Rails are the [Devise](https://github.com/plataformatec/devise) rails engine and the [Authlogic](https://github.com/binarylogic/authlogic) gem, along with a number of others.
 
 Security, especially in web applications, is a broad and detailed area. Security in your Rails application is covered in more depth in the [Ruby on Rails Security](https://guides.rubyonrails.org/security.html) Guide.
  
 Now that you've seen your first Rails application, you should feel free to update it and experiment on your own.
 
 Remember, you don't have to do everything without help. As you need assistance getting up and running with Rails, feel free to consult these support resources:
 
 The [Ruby on Rails Guides](https://guides.rubyonrails.org/index.html)
 The [Ruby on Rails mailing list](https://discuss.rubyonrails.org/c/rubyonrails-talk)
 The [#rubyonrails](irc://irc.freenode.net/#rubyonrails) channel on irc.freenode.net
  
 The easiest way to work with Rails is to store all external data as UTF-8. If you don't, Ruby libraries and Rails will often be able to convert your native data into UTF-8, but this doesn't always work reliably, so you're better off ensuring that all external data is UTF-8.
 
 If you have made a mistake in this area, the most common symptom is a black diamond with a question mark inside appearing in the browser. Another common symptom is characters like "Ã¼" appearing instead of "ü". Rails takes a number of internal steps to mitigate common causes of these problems that can be automatically detected and corrected. However, if you have external data that is not stored as UTF-8, it can occasionally result in these kinds of issues that cannot be automatically detected by Rails and corrected.
 
 Two very common sources of data that are not UTF-8:
 
     Your text editor: Most text editors (such as TextMate), default to saving files as UTF-8. If your text editor does not, this can result in special characters that you enter in your templates (such as é) to appear as a diamond with a question mark inside in the browser. This also applies to your i18n translation files. Most editors that do not already default to UTF-8 (such as some versions of Dreamweaver) offer a way to change the default to UTF-8. Do so.
     Your database: Rails defaults to converting data from your database into UTF-8 at the boundary. However, if your database is not using UTF-8 internally, it may not be able to store all characters that your users enter. For instance, if your database is using Latin-1 internally, and your user enters a Russian, Hebrew, or Japanese character, the data will be lost forever once it enters the database. If possible, use UTF-8 as the internal storage of your database.

