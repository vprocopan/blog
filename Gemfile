source "https://rubygems.org"

# Use Jekyll directly (not github-pages)
gem "jekyll", "~> 4.3.1"
gem "minima", "~> 2.5"  # Or change this to your preferred theme

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  # Add any custom plugins here
  # gem "jekyll-seo-tag"
end

# Optional: If using any Faraday-based plugin
gem "faraday", "~> 2.0"
gem "faraday-retry", "~> 2.2"

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", platforms: [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", platforms: [:jruby]
