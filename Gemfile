source "https://rubygems.org"
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#

# This will help ensure the proper Jekyll version is running.
# gem "jekyll", "~> 4.2.2"

# This is the default theme for new Jekyll sites. You may change this to anything you like.
# gem "minimal-mistakes-jekyll"

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed"
  gem 'jekyll-include-cache'
  gem 'jekyll-paginate'
  gem 'jekyll-sitemap'
  gem 'jekyll-gist'
  gem 'jekyll-archives'
  gem 'jekyll-seo-tag'
end

# If you want to use GitHub Pages, comment all three sections above and
# uncomment the section below. To upgrade, run `bundle update github-pages`.
gem "jekyll", "~> 3.7.3"
gem "github-pages", group: :jekyll_plugins
gem "jekyll-include-cache", group: :jekyll_plugins

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
