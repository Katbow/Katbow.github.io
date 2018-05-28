# frozen_string_literal: true

source "https://rubygems.org"

group :jekyll_plugins do
  gem "github-pages"
  gem "jekyll-feed", "~> 0.6"
  gem "jekyll-paginate", "~> 1.1.0"
end

require 'rbconfig'
if RbConfig::CONFIG['target_os'] =~ /darwin(1[0-3])/i
  gem 'rb-fsevent'
end
