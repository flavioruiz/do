#!/usr/bin/env rake
require 'bundler/setup'
require 'bundler/gem_tasks'
require 'rspec/core/rake_task'

%w(install release).each do |task|
  Rake::Task[task].enhance do
    sh "rm -rf pkg"
  end
end

desc "Bump version on github"
task :bump do
  if `git status -s`.chomp == ""
    puts "\e[31mNothing to commit (working directory clean)\e[0m"
  else
    version  = Bundler.load_gemspec(Dir[File.expand_path('../*.gemspec', __FILE__)].first).version
    sh "git add .; git commit -a -m \"Bump to version #{version}\""
  end
end

task :release => :bump

desc "Run complete application spec suite"
RSpec::Core::RakeTask.new("spec") do |t|
  t.skip_bundler = true
  t.pattern = './spec/**/*_spec.rb'
  t.rspec_opts = %w(-fs --color --fail-fast)
end

task :default => :spec
