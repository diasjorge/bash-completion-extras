HOME = File.expand_path('~')
BASH_COMPLETION_DIR = File.join(HOME, '.bash_completion.d/')

COMPLETION_SCRIPTS = Dir['bash_completion.d/*']
DOT_FILES = Dir['dot_files/*']

FORCE = ENV.include?("force") && (ENV["force"]=='true'||ENV["force"]=="1") ? true : false

desc "Install the package"
task :install do
  puts "INSTALLING"
  puts "================================================================"
  
  check_bash_completion_dir

  DOT_FILES.each do |file|
    target = File.join(HOME, ".#{File.basename(file)}")
    check_and_install(file, target)
  end

  COMPLETION_SCRIPTS.each do |file|
    target = File.join(BASH_COMPLETION_DIR, File.basename(file))
    check_and_install(file, target)
  end

  puts """
================================================================  
Installation completed. 

Please make sure to edit your #{File.join(HOME,".bashrc")} and add the following snippet:

  # My custom bash completions
  if [ -f ~/.bash_completion ]; then
      . ~/.bash_completion
  fi

Then load your scripts:

  . #{File.join(HOME,".bashrc")}
  """
end

def check_and_install(file, target)
  if File.exist?(target)
    file_exists = "File #{target} exists. "
    if FORCE == true
      puts file_exists << "Unlinking."
      File.unlink(target)
      link_file(file, target)
    else
      puts file_exists << "Use rake install force=true to override."
    end
  else
    link_file(file, target)
  end
  puts ""
end

def link_file(file, target)
  `ln -nis #{File.expand_path file} #{target}`
  puts "Installed #{file} in #{target}"
end

def check_bash_completion_dir
  unless File.exist?(BASH_COMPLETION_DIR)
    puts "Creating directory in #{BASH_COMPLETION_DIR}"
    Dir.mkdir(BASH_COMPLETION_DIR)
  end
end
