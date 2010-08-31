require 'rake'

FOLDERS = %w(colors ftdetect ftplugin indent syntax doc plugin autoload snippets macros after)
SCRIPTS = %w(personal tabular nerdtree vim-cucumber vim-rails vim-git vim-haml vim-scratch ack.vim)
DOTVIM = "#{ENV['HOME']}/.vim"

desc "Pull down submodules"
task :preinstall do
  puts `git submodule init`
  exec('git submodule update')
end

desc "Install the files into ~/.vim"
task :install do
  FileUtils.mkdir_p FOLDERS.map{|f| "#{DOTVIM}/#{f}" }

  SCRIPTS.each do |s|
    FOLDERS.each do |f|
      FileUtils.cp_r Dir["#{s}/#{f}/*"], "#{DOTVIM}/#{f}"
    end
  end
  FileUtils.cp "dotvimrc", ".vimrc"
  FileUtils.cp "dotgvimrc", ".gvimrc"
end

task :default => :preinstall
desc "Remove everything in ~/.vim"
task :uninstall do
  FileUtils.rm_rf DOTVIM
end

desc "Blow everything out and try again."
task :reinstall => [:uninstall, :install]
