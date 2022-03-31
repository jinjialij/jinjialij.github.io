---
layout: post
title: 'Run Github pages locally'
subtitle: 'Using Jekyll, on Windows'
date: 2022-03-29
author: 'Jiali'
header-img: 'img/post-bg-githubpages.png'
tags:
  - Github Pages
  - Jekyll
---

# Run Github pages locally using Jekyll

The [Github pages doc](https://docs.github.com/en/enterprise-server@3.1/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll) have a detailed instructions on how to create a GitHub Pages site with Jekyll. However, I encounter problems when testing the site locally. (Step 14 in `Creating your site`)

## Steps I did

To create a new jekyll project, run `jekyll new .`

In Gemfile, add # to line `gem "jekyll", "~> 4.2.2"`, to comment it; add `gem "github-pages", "~> 225", group: :jekyll_plugins`

Run Jekyll locally: run `bundle exec jekyll serve`

But I encounter errors as below:

```txt
Configuration file: C:/Users/jekyll-github/_config.yml
            Source: C:/Users/jekyll-github
       Destination: C:/Users/jekyll-github/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 2.585 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'C:/Users/jekyll-github'
                    ------------------------------------------------
      Jekyll 4.2.2   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `<top (required)>'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:179:in `require_relative'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:179:in `setup'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:100:in `process'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `block in process_with_graceful_fail'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `each'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `process_with_graceful_fail'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:86:in `block (2 levels) in init_with_program'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `block in execute'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `each'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `execute'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/program.rb:44:in `go'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary.rb:21:in `program'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/exe/jekyll:15:in `<top (required)>'
        from C:/Ruby31-x64/bin/jekyll:25:in `load'
        from C:/Ruby31-x64/bin/jekyll:25:in `<main>'
```

## Solution

Add `gem 'wdm', '>= 0.1.1' if Gem.win_platform?` in the Gemfile, delete `Gemfile.lock` and run `bundle install`. It will install wdm 0.1.1.

Before exec again, run `bundle add webrick` to solve :

```txt
in `require': cannot load such file -- webrick (LoadError)` problem
```

Now run `bundle exec jekyll serve` and you are set.

The complete Gemfile is like this:

```Ruby
source "https://rubygems.org"
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
# gem "jekyll", "~> 4.2.2"
# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.5"
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
gem "github-pages", "~> 225", group: :jekyll_plugins
# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
# gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem 'wdm', '>= 0.1.1' if Gem.win_platform?

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

gem "webrick", "~> 1.7"
```

## More Reference

[Up and Running with GitHub Pages, Part 3, Blogging with Jekyll](https://www.youtube.com/watch?v=EmSrQCDsMv4)

[Load error: cannot load such file – webrick](https://talk.jekyllrb.com/t/load-error-cannot-load-such-file-webrick/5417/6)
