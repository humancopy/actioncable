require 'rake/testtask'
require 'pathname'
require 'action_cable'
require 'blade'

dir = File.dirname(__FILE__)

task :default => :test

task :package => "assets:compile"

Rake::TestTask.new do |t|
  t.libs << "test"
  t.test_files = Dir.glob("#{dir}/test/**/*_test.rb")
  t.warning = true
  t.verbose = true
  t.ruby_opts = ["--dev"] if defined?(JRUBY_VERSION)
end

namespace :test do
  task :isolated do
    Dir.glob("test/**/*_test.rb").all? do |file|
      sh(Gem.ruby, '-w', '-Ilib:test', file)
    end or raise "Failures"
  end

  task :integration do
    if ENV['CI']
      Blade.start(interface: :ci)
    else
      Blade.start(interface: :runner)
    end
  end
end

namespace :assets do
  desc "Compile Action Cable assets"
  task :compile do
    Blade.build
  end
end
