require 'html-proofer'
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec)

desc 'Run the Markdown specs and HTML Proofer'
task :ci do
  sh 'bundle exec jekyll clean'
  sh 'bundle exec jekyll build -d _site/test --baseurl /test'

  sh 'grunt test'
  sh 'scripts/check_json.py -v'
  Rake::Task['spec'].invoke
  Rake::Task['check_html'].invoke
end

desc 'Check links and html without caching'
task :check_html do
  HTMLProofer.check_directory('./_site', cache: { timeframe: '1w' },
                                         check_html: true,
                                         http_status_ignore: [0]).run
end

desc 'Run the site locally on localhost:4000'
task :dev do
  sh 'bundle exec jekyll clean'
  sh 'bundle exec jekyll build'
  sh 'bundle exec jekyll serve --watch --drafts'
end

task default: :ci
