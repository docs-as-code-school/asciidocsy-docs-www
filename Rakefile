require 'rake'
require 'safe_yaml'
require 'fileutils'
require 'jekyll'
require 'asciidoctor'

SafeYAML::OPTIONS[:deserialize_symbols] = true
SafeYAML::OPTIONS[:default_mode] = :safe

@config = YAML.load_file("_config.yml")
@src_git_ref = 'main'
@src_path    = 'subjects/asciidocsy'

# BUILD TASKS

desc "jekyll build"
task :build do
  clean() unless args('keep')
  migrate_subject_files() unless args('cached')
  puts "Building site..."
  system "bundle exec jekyll build"
end

desc "build and serve the site"
task :serve do
  clean() unless args('keep')
  migrate_subject_files() unless args('cached')
  puts "Building and serving site..."
  system "bundle exec jekyll serve"
end

desc "generate and push Algolia index files"
task :index do
  clean()
  migrate_subject_files()
  puts "Generating search-index data..."
  ['pages','topics'].each do |idx|
    system "bundle exec jekyll algolia --config _config.yml,_data/configs/search-index-#{idx}.yml"
  end
end

# COMMON TASKS

desc "wipe the scratch directories locally"
task :clean do
  clean()
end

desc "pull and cache dependency source files"
task :cache do
  migrate_subject_files()
end

task :help do
  puts "Usage: rake <command> [<arguments>]"
  puts "       rake build"
  puts "       rake serve"
  puts "       rake index"
  puts "       rake clean"
  puts "       rake cache"
end

# PROC FUNCTIONS
desc "return true if command arguments include specified string"
def args string
  return false if ARGV.select{|a| a == string}.empty?
  true
end

desc "remove all scratch directories"
def clean
  paths = []
  paths.push(@config['destination'])
  paths.push('.jekyll-cache')
  paths.concat(targets_map())
  puts "Performing wipe..."
  %(rm -rf #{paths.join(' ')})
end

desc "Copy subject source files to untracked 'scratch' paths"
def migrate_subject_files
  pull_subject_files()
  puts "Copying source files..."
  paths = @config['subjects']['asciidocsy']['paths']
  paths.each do |path|
    p = path[1]
    src = Dir.glob(p['source'])
    FileUtils.mkdir_p(p['target']) unless File.directory?(p['target'])
    FileUtils.cp_r(src, p['target'])
    FileUtils.cp_r(src, p['target'])
  end
  FileUtils.cp("#{@src_path}/README.adoc", paths['snippets']['target'])
end

desc "Checkout and pull submodule"
def pull_subject_files
  Dir.chdir("#{@src_path}"){
    puts "Checking out and pulling subject submodule..."
    %x(git checkout #{@source_git_ref})
    %x(git pull)
  }
end

def targets_map
  @config['subjects']['asciidocsy']['paths'].map{|p| p[1]['target'] }
end

def sources_map
  @config['subjects']['asciidocsy']['paths'].map{|p| p[1]['source'] }
end
