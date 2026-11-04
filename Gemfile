source "https://rubygems.org"

# Mirrors the GitHub Pages build environment. Run Jekyll through Bundler:
#   bundle exec jekyll serve -l
gem "github-pages", group: :jekyll_plugins

group :jekyll_plugins do
  gem "jekyll-sitemap"
end

# Ruby 3+ no longer bundles webrick, which `jekyll serve` needs.
gem "webrick", "~> 1.8"

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end
