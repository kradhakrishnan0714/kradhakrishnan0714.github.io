source "https://rubygems.org"

# NOTE: GitHub Pages builds this site with its own pinned gem set and ignores
# this Gemfile. It exists purely for local previewing (`bundle exec jekyll serve`).
# We use modern Jekyll here because the `github-pages` gem pins Liquid 4.0.3,
# which calls `tainted?` — removed in Ruby 3.2+ and unavailable on current Rubies.
gem "jekyll", "~> 4.3"

group :jekyll_plugins do
  gem "jekyll-remote-theme"
  gem "jekyll-include-cache"
  gem "jekyll-seo-tag"
  gem "jekyll-feed"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-paginate"
end

# Required on Ruby 3.x+ for `jekyll serve`.
gem "webrick", "~> 1.8"

# Stdlib gems unbundled in Ruby 3.4+/4.0.
gem "csv"
gem "base64"
gem "bigdecimal"
gem "logger"
gem "ostruct"
