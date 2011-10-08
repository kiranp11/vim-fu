require 'rake'


SCRIPTS_WITH_RAKE = %w(Command-T)
FOLDERS = %w(colors ftdetect ftplugin indent syntax doc plugin autoload snippets macros after ruby)
SCRIPTS = %w(personal Tagbar vim-rvm Headlights vim-coffee-script tabular nerdtree vim-cucumber vim-rails vim-fugitive vim-haml vim-scratch ack.vim snipmate.vim vim-spec tcomment_vim vim-bufonly vim-endwise vim-surround vim-yankring supertab vim-unimpaired Slimv vimclojure vim-rake vim-javascript vim-autoclose vim-ruby-refactoring matchit cscope gundo.vim vim-conque vim-repeat vim-space vim-colors-solarized) + SCRIPTS_WITH_RAKE
DOTVIM = "#{ENV['HOME']}/.vim"

desc "Pull down submodules"
task :preinstall do
  puts `git submodule init`
  exec('git submodule update')
end

desc "Get latest on all the submodules"
task :update do
  scripts_without_version_control = []
  SCRIPTS.each do |f|
    if File.directory? "#{f}/.git"
      Dir.chdir f
      system "git pull origin master"
      Dir.chdir '..'
    elsif File.directory? "#{f}/.hg"
      system "hg pull #{f}"
    else
      scripts_without_version_control << f
    end
  end
  puts "Scripts without version control: #{scripts_without_version_control.join(', ')}"
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
