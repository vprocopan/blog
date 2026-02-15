source "https://rubygems.org"

# Local development stack (avoids hard dependency on github-pages gem)
gem "jekyll", "~> 4.4"

# Optional: You can keep minima if you're customizing it manually,
# but GitHub Pages already includes it by default.
# You may remove this line if not needed:
gem "minima", "~> 2.5"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  # Add any custom plugins here
  # gem "jekyll-seo-tag"
end

# Windows and JRuby compatibility
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# File watching improvements for Windows
gem "wdm", "~> 0.1.1", platforms: [:mingw, :x64_mingw, :mswin]

# Lock http_parser for JRuby compatibility
gem "http_parser.rb", "~> 0.6.0", platforms: [:jruby]
