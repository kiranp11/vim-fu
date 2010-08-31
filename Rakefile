require 'rake'

FOLDERS = %w(colors ftdetect ftplugin indent syntax doc plugin autoload snippets macros after)
SCRIPTS = %w(personal tabular nerdtree vim-cucumber vim-rails fugitive vim-haml vim-scratch ack.vim snipmate.vim project vim-spec tcomment_vim vim-bufonly vim-endwise minibufexpl vim-surround yankring)
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
