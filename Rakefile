task :default => :server

desc 'Build site with Jekyll'
task :build do
  jekyll('--no-auto')
end

desc 'Start server with --auto'
task :server do
  jekyll('--server --auto')
end

desc 'Build and deploy'
task :deploy => :build do
  sh 'rsync -rtzh --progress --delete _site/ jamiew@txd:~/domains/irenepolnyi.com/web/public'
end

def jekyll(opts = '')
  sh 'rm -rf _site'
  sh 'jekyll ' + opts
end