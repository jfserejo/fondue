Fondue
======

*__This is an hack to set Compass + Susy Grid Framework on Heroku__*

### Step 1: Sign Up

Sign up for a Heroku account, if you don’t already have one.

### Step 2: Heroku Toolbelt

Grab & Install the Heroku [Toolbelt](https://toolbelt.heroku.com). (...) "The toolbelt contains the Heroku client, a command-line tool for creating and managing Heroku apps; Foreman, an easy option for running your apps locally; and Git, the revision control system needed for pushing applications to Heroku."

### Step 3: Login & Create an App

After installing the Toolbelt, you’ll have access to the heroku command from your command shell. Authenticate using the email address and password you used when creating your Heroku account:

If you have previously uploaded a key to Heroku, we assume you will keep using it and do not prompt you about creating a new one during login. If you would prefer to create and upload a new key after login, simply run: heroku keys:add

    $ heroku login
    Enter your Heroku credentials.
    Email: someuser@example.com
    Password: 
    Could not find an existing public key.
    Would you like to generate one? [Yn] 
    Generating new SSH public key.
    Uploading ssh public key /Users/someuser/.ssh/id_rsa.pub

Press enter at the prompt to upload your existing ssh key or create a new one, used for pushing code later on.

Create the app:

    $ heroku create
    Creating some-random-funny-name-997... done
    Created http://some-random-funny-name-997.herokuapp.com/ | git@heroku.com:some-random-funny-name-997.git
    Git remote heroku added

### Step 4: Deploy the application

**4.1 Creating site tree:**

    $ mkdir -p myapp/{public/assets/{icons,fonts,stylesheets,javascripts,images},lib/{docs,scss}}
    
    $ touch myapp/{config.ru,Gemfile,Gemfile.lock,public/index.html}
    
    /
    Gemfile
    Gemfile.lock
    config.ru
    /lib/
      	docs/
    	scss/
    
    /public/
    	assets/
    		fonts/
    		icons/
    		images/
    		javascripts/
    		stylesheets/
    	index.html

**4.2 Edit the Gemfile:**

    source :rubygems
    
    gem 'rack'
    gem 'susy'
    gem 'compass'

**4.3 Run Bundler:**

    $ bundle install
    Using rack (1.4.1)
    Your bundle is complete! Use `bundle show [gemname]` to see where a bundled gem is installed.

**4.4 File server configuration:**

Rack must be told which files it should serve as static assets. Do this by editing the config.ru file so it contains the following:

    use Rack::Static, 
      :urls => ["/assets/fonts", "/assets/icons", "/assets/images", "/assets/javascripts", "/assets/stylesheets"],
      :root => "public"
    
    run lambda { |env|
      [
        200, 
        {
          'Content-Type'  => 'text/html', 
          'Cache-Control' => 'public, max-age=86400' 
        },
        File.open('public/index.html', File::RDONLY)
      ]
    }

**4.5 Test locally:**

To run your site locally and view any modifications before deploying to Heroku run the following command in the site directory:

    $ rackup
    >> Thin web server (v1.5.0 codename Knife)
    >> Maximum connections set to 1024
    >> Listening on 0.0.0.0:9292, CTRL+C to stop

Go to your browser and open [http://localhost:9292](http://localhost:9292) to see your static site loaded. As you make changes to the site you only need to reload your browser to view the changes – you don’t have to bounce the server.

**4.6 Store on git:**

Before deploying your site to Heroku you will need to store your code in the Git version control system which was installed for you as part of the Heroku Toolbelt. When inside the site root directory execute the following commands:

    $ git init
    $ git add .
    $ git commit -m "Initial static site template app"

**4.7 Use git to deploy to Heroku as well:**

    $ git push heroku master
    Counting objects: 6, done.
    Delta compression using up to 4 threads.
    ...
    
    -----> Heroku receiving push
    -----> Ruby app detected
    ...
    -----> Launching... done, v1
           http://some-random-funny-name-997.herokuapp.com deployed to Heroku

At this point your site is deployed to Heroku and has been given an auto-generated URL. Open your site using the heroku open command:

    $ heroku open
    some-random-funny-name-997 ... done

### Step 5 Updating:

When making changes to your site, edit the necessary files locally, commit them to Git, and push to Heroku to see the changes deployed.

If needed:

    $ git add * (or point the files to add)

then:

    $ git commit -m "Commit Update 1"
    $ git push heroku master

### Step 6 Compass:


**6.1 Edit the config.rb file:**

    require 'susy'
    
    http_path = "public"
    css_dir = "public/assets/stylesheets"
    sass_dir = "/lib/scss"
    images_dir = "public/assets/images"
    javascripts_dir = "public/assets/javascripts"
    
    # output_style = :expanded or :nested or :compact or :compressed
    output_style = :nested
    
    # To enable relative paths to assets via compass helper functions. Uncomment:
    # relative_assets = true
    
    # To disable debugging comments that display the original location of your selectors. Uncomment:
    line_comments = false
    
    # If you prefer the indented syntax, you might want to regenerate this
    # project again passing --syntax sass, or you can uncomment this:
    # preferred_syntax = :sass
    # and then run:
    # sass-convert -R --from scss --to sass sass scss && rm -rf sass && mv scss sass

**6.2 Now compass can watch changes:**

    $ compass watch myapp 
    
    ...
    Dear developers making use of FSSM in your projects,
    FSSM is essentially dead at this point. Further development will
    be taking place in the new shared guard/listen project. Please
    let us know if you need help transitioning! ^_^b
    - Travis Tilley
    
    >>> Compass is watching for changes. Press Ctrl-C to Stop.
    >>> Change detected at 22:45:52 to: dummy.scss
       create myapp/public/assets/stylesheets/dummy.css 
    overwrite myapp/public/assets/stylesheets/screen.css 
    ^C

**6.3 Update the changes to your site ...**

*(Same as Step 5)*

If needed:

    $ git add * (or point the files to add)

then:

    $ git commit -m "Commit Update 2"
    $ git push heroku master

...

### [See: Heroku Deploying with Git](https://devcenter.heroku.com/articles/git)
