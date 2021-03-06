Notes on OSX installation
==========================

In order to work on LocalSupport on Mac, please fork and clone the project.

1. Install [RVM](https://rvm.io/).
1. Install [Ruby 2.3.1](https://rvm.io/rubies/installing).
1. Fork the http://github.com/AgileVentures/LocalSupport repo (fork button at top right of github web interface).
1. Clone the new forked repo onto your dev machine.
1. `cd LocalSupport`.
1. Install Qt webkit headers.
Install Homebrew (if you don't have it already). If you do have Homebrew, run `brew update` and after brew is installed or updated, type `brew install qt@5.5`
This will allow you to install capybara-webkit -v '1.0.0' successfully.
If you are still unable to successfully install the caybara-webkit try the following.  First `brew uninstall qt@5.5` to uninstall previous installation, then re-install using 
`brew install qt@5.5 --with-qtwebkit`, and finally `brew link qt@5.5`. 
You may also need to update your paths. If using standard bash `echo 'export PATH="/usr/local/opt/qt@5.5/bin:$PATH"' >> ~/.bashrc`, or if using zsh 
`echo 'export PATH="/usr/local/opt/qt@5.5/bin:$PATH"' >> ~/.zshrc`
1. Install postgreSQL.
Type `psql` into command line. Then you should see this:

        psql (9.3.4)
        Type "help" for help.
        username=#
Next to that type `ALTER ROLE "postgres" WITH CREATEDB`.  (You may need to create this role, see [issues](issues.md) for help.)
1. `git checkout develop`.
1. Run `bundle install` to get the gems.
1. Run `npm install` to get the javascript dependencies.
1. Run `bower install` to get remaining dependencies.
1. Run the following to get the database set up and import seed data (**Note:** `db:setup` is a custom task that invokes all required import and seeds. )

    ```ruby
    bundle exec rake db:create
    bundle exec rake db:migrate
    bundle exec rake db:setup
    ```

If you hit problems, ask us on Slack chat.

[Note that rvm can be extremely helpful for managing ruby versions.  [Installing rvm on Ubuntu](https://www.digitalocean.com/community/articles/how-to-install-ruby-on-rails-on-ubuntu-12-04-lts-precise-pangolin-with-rvm)]

Debian 7 users can follow these instructions to launch Xvfb as daemon : [[Xvfb-on-Debian-7]]

## Run locally
After that, in principle you can run rails server and see that app running locally.

The db/seeds.rb task that you ran added some organizations and a test user that you can experiment with. Read that file for more information.

## Run tests

Also you should run the specs and cucumber features to make sure your installation is solid. For confidence, you shall prepare the test database first by running
`rake db:test:prepare`(not needed in ruby starting from 2.0.0).

Before running the tests you should create a file named `config/application.yml` and add the following line:

```yaml
DOIT_HOST: 'http://api.qa2.do-it.org/v2'
```

then run tests using following commands:

    bundle exec rake spec
    bundle exec rake cucumber

and then when you start any BDD or TDD ensure autotest is running in the background:

    bundle exec rake autotest

After that try running bundle install again.

Issues
-------

* If you are having issues with PostgresQL - see [PostgreSQL install instructions](issues.md#postgresql-install)
* If you are having issues installing Qt webkit headers- see [capybara-webkit gem](issues.md#capybara-webkit-gem)
