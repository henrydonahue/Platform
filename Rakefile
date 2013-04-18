# Add your own tasks in files placed in lib/tasks ending in .rake,
# for example lib/tasks/capistrano.rake, and they will automatically be available to Rake.

require File.expand_path('../config/application', __FILE__)
require 'rake'
require 'rake/dsl_definition'
require 'rubygems'

include Rake::DSL

begin
  require 'cucumber'
  require 'cucumber/rake/task'
  require 'rspec/core/rake_task'

  desc "Run Cuke"
  Cucumber::Rake::Task.new(:cuke) do |t|
    t.profile = "default"
  end

  namespace :platform do
    desc "Run the equivalent of the platform-ci-full build on jenkins. It includes all cucumber specs that depend on javascript"
    Cucumber::Rake::Task.new(:ci_full) do |t|
      t.profile = "default"
    end
  end

  namespace :test do
    namespace :pre do
      desc "Run pre checkin tests"
      task :checkin => [:spec, :cuke, "jasmine:ci", :jslint]
      task :parallel => ["parallel:spec", "parallel:features", "jasmine:ci", :jslint]
    end
  end

rescue LoadError
end

PurposePlatform::Application.load_tasks

task :default => 'test:pre:checkin'