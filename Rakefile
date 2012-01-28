require "rake/testtask"
require "resque/tasks"

$LOAD_PATH.push("./") unless $LOAD_PATH.include?("./")

task :test => ["test:units", "test:integrations", "test:coffeescripts"]

namespace :test do
  Rake::TestTask.new(:units) do |task|
    task.libs << "test"
    task.test_files = FileList["test/unit/*"]
  end

  # TODO(caleb): Use a better name for this task.
  desc "Run the coffeescript unit tests with shoulda.js."
  task :coffeescripts do
    puts `node node_modules/jasmine-node/lib/jasmine-node/cli.js --coffee test/coffeescript`
  end

  Rake::TestTask.new(:integrations) do |task|
    task.libs << "test"
    task.test_files = FileList["test/integration/*"]
  end
end

namespace :resque do
  # The resque:work task is defined by the Resque gem. Before running it, we need to require the workers.
  task :work => :require_resque_tasks

  # These tasks must be defined for Resque to be able to run tasks on these queues.
  task :require_resque_tasks do
    Dir.glob("resque_jobs/*.rb").each { |file_name| require file_name }
  end
end