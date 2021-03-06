require "bundler/setup"

task :default => [:list]

desc "Lists all the tasks"
task :list do
  puts "Tasks: \n- #{(Rake::Task.tasks).join("\n- ")}"
end

desc "Checks for required dependencies."
task :check do
  environment_vars = [
    'OPSCODE_USER',
    'OPSCODE_ORGNAME',
    'KNIFE_CLIENT_KEY_FOLDER',
    'KNIFE_VALIDATION_KEY_FOLDER',
    'KNIFE_CHEF_SERVER',
    'KNIFE_COOKBOOK_COPYRIGHT',
    'KNIFE_COOKBOOK_LICENSE',
    'KNIFE_COOKBOOK_EMAIL',
    'KNIFE_CACHE_PATH'
  ]
  errors = []
  environment_vars.each do |var|
    if ENV[var].nil?
      errors.push(" - \e\[31m#Variable: {var} not set!\e\[0m\n")
    else
      puts " - \e\[32mVariable: #{var} set to \"#{ENV[var]}\"\e\[0m\n"
    end
  end

  file_list = [
  "#{ENV['KNIFE_CLIENT_KEY_FOLDER']}/#{ENV['OPSCODE_USER']}.pem",
  "#{ENV['KNIFE_VALIDATION_KEY_FOLDER']}/#{ENV['OPSCODE_ORGNAME']}-validator.pem",
]

  file_list.each do |file|
    if File.exist?(file)
      puts " - \e\[32mFile: \"#{file}\" found.\e\[0m\n"
    else
      errors.push(" - \e\[31mRequired file: \"#{file}\" not found!\e\[0m\n")
    end
  end

  if system("command -v virtualbox >/dev/null 2>&1")
    puts " - \e\[32mProgram: \"virtualbox\" found.\e\[0m\n"
  else
    errors.push(" - \e\[31mProgram: \"virtualbox\" not found!\e\[0m\n")
  end

  if system("command -v vagrant >/dev/null 2>&1")
    puts " - \e\[32mProgram: \"vagrant\" found.\e\[0m\n"
    %x{vagrant --version}.match(/\d+\.\d+\.\d+/) { |m|
      if Gem::Version.new(m.to_s) < Gem::Version.new('1.1.0')
        errors.push(" -- \e\[31mVagrant version \"#{m.to_s}\" is too old!\e\[0m\n")
      else
        puts " - \e\[32mVagrant \"#{m.to_s}\" works!\e\[0m\n"
      end
    }
  else
    errors.push(" - \e\[31mProgram: \"vagrant\" not found!\e\[0m\n")
  end

  unless errors.empty?
    puts "The following errors occured:\n#{errors.join()}"
  end
end

desc "Builds the package."
task :build do
  puts "Build TBD"
end

desc "Fires up the Vagrant Box"
task :start do
  sh "vagrant up"
end
