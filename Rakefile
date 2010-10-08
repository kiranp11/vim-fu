require 'rake'

SCRIPTS_WITH_RAKE = %w(Command-T)
FOLDERS = %w(colors ftdetect ftplugin indent syntax doc plugin autoload snippets macros after ruby)
SCRIPTS = %w(personal tabular nerdtree vim-cucumber vim-rails vim-fugitive vim-haml vim-scratch ack.vim snipmate.vim project vim-spec tcomment_vim vim-bufonly vim-endwise vim-surround yankring vim-fuzzyfinder supertab rvm.vim vim-unimpaired vim-autocomplpop vim-l9) + SCRIPTS_WITH_RAKE
DOTVIM = "#{ENV['HOME']}/.vim"

desc "Pull down submodules"
task :preinstall do
  puts `git submodule init`
  exec('git submodule update')
end

desc "Get latest on all the submodules"
task :update do
  SCRIPTS.each do |f|
    if File.directory? "#{f}/.git"
      Dir.chdir f
      system "git pull origin master"
      Dir.chdir '..'
    elsif File.directory? "#{f}/.hg"
      system "hg pull #{f}"
    else
      puts "didn't find #{f}"
    end
  end
end

desc "Install the files into ~/.vim"
task :install do
  FileUtils.mkdir_p FOLDERS.map{|f| "#{DOTVIM}/#{f}" }
  FileUtils.mkdir_p "#{DOTVIM}/tmp"

  SCRIPTS_WITH_RAKE.each do |f|
    Dir.chdir f
    system 'rake make'
    Dir.chdir '..'
  end
  SCRIPTS.each do |s|
    FOLDERS.each do |f|
      FileUtils.cp_r Dir["#{s}/#{f}/*"], "#{DOTVIM}/#{f}"
    end
  end
  FileUtils.cp "dotvimrc", "#{ENV['HOME']}/.vimrc"
  FileUtils.cp "dotgvimrc", "#{ENV['HOME']}/.gvimrc"
end

task :default => :preinstall

desc "Remove everything in ~/.vim"
task :uninstall do
  FileUtils.rm_rf DOTVIM
end

desc "Blow everything out and try again."
task :reinstall => [:uninstall, :install]
