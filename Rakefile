task :default => :server

desc 'Build site with Jekyll'
task :build do
  jekyll('--no-auto')
end

desc 'Start server with --auto'
task :server do
  jekyll('--server --auto')
end

desc 'Deploy publicly'
task :deploy do
  sh 'git push release master:gh-pages'
end

def jekyll(opts = '')
  sh 'rm -rf _site'
  sh 'jekyll ' + opts
end
