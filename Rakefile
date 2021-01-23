# frozen_string_literal: true

require "rake/testtask"
require "rake/extensiontask"
require "bundler/gem_tasks"
require 'ffi'
# Build tasks
p ENV['RGEO_SKIP_C_EXT']
p ENV['GEOS_LIBRARY_PATH']

p module TestGeos
  extend FFI::Library
  ffi_lib(ENV['GEOS_LIBRARY_PATH'])
end

if RUBY_DESCRIPTION.match(/^jruby/) || ENV["RGEO_SKIP_C_EXT"] == 'true'
  task :compile
else
  Rake::ExtensionTask.new "geos_c_impl" do |ext|
    ext.lib_dir = "lib/rgeo/geos"
  end
end

task :clean do
  clean_files = %w[pkg tmp] +
    Dir.glob("ext/**/Makefile*") +
    Dir.glob("ext/**/*.{o,class,log,dSYM}") +
    Dir.glob("**/*.{bundle,so,dll,rbc,jar}") +
    Dir.glob("**/.rbx")

  clean_files.each { |path| rm_rf path }
end

Rake::TestTask.new(:test) do |t|
  t.libs << "test"
  t.libs << "lib"
  t.test_files = FileList["test/**/*_test.rb"]
end

task test: :compile
task default: [:clean, :test]
