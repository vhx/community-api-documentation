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
  # TODO freak out if not in gh-pages. This double-push doesn't behave well
  # sh 'git push origin master:gh-pages'
  sh 'git push origin gh-pages'
end

def jekyll(opts = '')
  sh 'rm -rf _site'
  sh 'jekyll ' + opts
end
