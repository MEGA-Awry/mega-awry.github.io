# Local Development

To compile this website locally, install [Jekyll](https://jekyllrb.com/docs/installation/) (which requires you to have [Ruby](https://www.ruby-lang.org/en/downloads/)).

Next, install the Ruby gem `bundler`:
```
gem install bundler
```

Afterwards, install the other Ruby dependencies for this project using the Gemfile. For this, run the following command in the root of this project.
```
bundle install
```

Finally, you can (re)build the website and run it locally with the following command:
```
bundle exec jekyll serve
```

The website should now be accessible under [http://127.0.0.1:4000](http://127.0.0.1:4000).
