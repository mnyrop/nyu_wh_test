require 'html-proofer'
require 'rake'
include FileUtils

desc 'run htmlproofer, rspec if exists'
task :test do
  opts = {
    :check_external_hash => true,
    :allow_hash_href => true,
    :disable_external => true,
    :empty_alt_ignore => true,
    :only_4xx => true,
    :verbose => true
  }
  HTMLProofer.check_directory('./_site', opts).run
  sh 'bundle exec rspec' if File.exist? '.rspec'
end


desc 'push built site to host'
task :deploy do
  if ENV.fetch('CI', false)
    BRANCH = ENV['TRAVIS_BRANCH']
    if BRANCH == 'main'
      COMMIT_MSG  = "Site updated via #{ENV['TRAVIS_COMMIT']}".freeze
      USER        = ENV['GIT_USER'].freeze
      PW          = ENV['GIT_PW'].freeze
      TARGET      = ENV['GIT_TARGET'].freeze
      REMOTE      = "git://#{USER}:#{PW}@#{TARGET}".freeze
      puts "Deploying to host from Travis-CI"

      Dir.chdir '_site'
      system 'git init'
      system "git checkout -b main"
      system "git add . && git commit -m '#{COMMIT_MSG}'"
      system "git remote add server #{REMOTE}"
      system "git push server main --force"
    else
      puts "This task only runs on the main branch. Skipping for #{BRANCH}."
    end
  else
    puts 'Not on Travis-CI. Skipping.'
  end
end
