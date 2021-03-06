task default: %w[test]

task :test do
  puts "\nBuilding project"
  try "middleman build"
end

task :deploy do
  puts "\nCopying GitHub-specific files"
  if (Dir.exists?('./github'))
    try "cp -rv ./github/* ./build/"
  end

  puts "\nDeploying to GitHub"
  try "middleman deploy"
end

namespace :travis do
  task :script do
    Rake::Task["test"].invoke
  end

  task :after_success do
    puts "\nRunning Travis Deployment"
    puts "\nSetting up Git access"
    try "echo \"https://${GH_TOKEN}:x-oauth-basic@github.com\" > ~/.git-credentials"
    try "git config --global user.name ${GH_USER}"
    try "git config --global user.email ${GH_EMAIL}"
    try "git config --global credential.helper store"
    try "git remote set-url origin \"https://github.com/elhu/elhu.github.io.git\""

    Rake::Task["deploy"].invoke
  end
end

## Helper so we fail as soon as a command fails.
def try(command)
  system command
  if $? != 0 then
    raise "Command: `#{command}` exited with code #{$?.exitstatus}"
  end
end
