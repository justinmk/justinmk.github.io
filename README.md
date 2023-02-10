https://github.com/github/pages-gem

## Run

    bundle config set --local path .bundle
    bundle install
    bundle exec jekyll serve --watch --livereload

## Update deps

    bundle config set --local path .bundle
    rm Gemfile.lock
    bundle update webrick
    bundle update github-pages
