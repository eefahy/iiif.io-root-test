require 'html-proofer'
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec)

def jekyll(cmd)
  sh "bundle exec jekyll #{cmd}"
end

def build_site
  jekyll 'clean'
  jekyll 'build -d _site/test --baseurl /test'
end

desc 'Run the Markdown specs and HTML Proofer'
task :ci do
  build_site
  sh 'grunt test'
  sh 'scripts/check_json.py -v'
  Rake::Task['spec'].invoke
  Rake::Task['check_html'].invoke
end

desc 'Check links and html without caching'
task :check_html do
  HTMLProofer.check_directory('./_site', cache: { timeframe: '1w' },
                                         check_html: true,
                                         only_4xx: true).run
end

desc 'Run the site locally on localhost:4000'
task :dev do
  build_site
  jekyll 'serve --watch --drafts'
end

task default: :ci
