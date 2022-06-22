# My blog on Githubpages

I customized from the boilerplate of [Hux Blog](https://github.com/Huxpro/huxpro.github.io)

# To run the code locally

## For macos apple chip
- Install Homebrew
- Install Ruby: [Install Ruby 3.1 · macOS](https://mac.install.guide/ruby/13.html)
- Install the jekyll and bundler gems
    - `gem install jekyll bundler`
- Installed dependencies in the Gemfile
    - `bundle install --path vendor/bundle`
    - `bundle add webrick`
- Run script to serve the website
    - `bundle exec jekyll serve --watch`
