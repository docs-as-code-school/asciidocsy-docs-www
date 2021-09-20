source 'https://rubygems.org'
# gem "clide", path: "../clide-mvp"
gem "rake", "~> 12.3"
# tag::jekyll-plugins[]
gem 'jekyll', '~> 4.2'
group :jekyll_plugins do
  gem 'asciidocsy'
  gem 'jekyll-algolia', "~> 1.4"
end
# end::jekyll-plugins[]

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
