https://github.com/github/pages-gem

## Run

    gem update
    gem install bundler
    bundle config set --local path .bundle
    bundle install
    bundle exec jekyll serve --watch --livereload

### Fix broken gem/bundle/idk

If you see an error like this:

    /usr/local/opt/ruby/bin/bundle:25:in `load': cannot load such file -- /usr/local/lib/ruby/gems/3.2.0/gems/bundler-2.4.6/exe/bundle (LoadError)
    from /usr/local/opt/ruby/bin/bundle:25:in `<main>'

The broken path is probably coming from the lockfile, so remove it.

    rm Gemfile.lock
    bundle install

## Update deps

    gem update
    gem install bundler
    bundle config set --local path .bundle
    rm Gemfile.lock
    bundle update webrick
    bundle update github-pages
