require "bundler"
Bundler.setup

require 'rake'
require 'rake/testtask'

namespace :display do
  task :notice do
    puts
    puts "To run tests you must supply the adapter, see rake -T for more information."
    puts
  end
end
task default: ["display:notice"]

ADAPTERS = %w(mysql2 jdbcmysql jdbcpostgresql postgresql sqlite3 seamless_database_pool mysql2spatial spatialite postgis).freeze
ADAPTERS.each do |adapter|
  namespace :test do
    desc "Runs #{adapter} database tests."
    Rake::TestTask.new(adapter) do |t|
      # FactoryGirl has an issue with warnings, so turn off, so noisy
      # t.warning = true
      t.test_files = FileList["test/adapters/#{adapter}.rb", "test/*_test.rb", "test/active_record/*_test.rb", "test/#{adapter}/**/*_test.rb"]
    end
    task adapter
  end
end

begin
  require 'rcov/rcovtask'
  adapter = ENV['ARE_DB']
  Rcov::RcovTask.new do |test|
    test.libs << 'test'
    test.pattern = ["test/adapters/#{adapter}.rb", "test/*_test.rb", "test/#{adapter}/**/*_test.rb"]
    test.verbose = true
  end
rescue LoadError
  task :rcov do
    abort "RCov is not available. In order to run rcov, you must: sudo gem install rcov"
  end
end

require 'rdoc/task'
Rake::RDocTask.new do |rdoc|
  version = File.exist?('VERSION') ? File.read('VERSION') : ""

  rdoc.rdoc_dir = 'rdoc'
  rdoc.title = "activerecord-import #{version}"
  rdoc.rdoc_files.include('README*')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

require 'rubocop/rake_task'
RuboCop::RakeTask.new