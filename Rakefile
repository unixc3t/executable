task :default => [:test]

desc "run unit tests (needs rubytest)"
task :test do
  sh "rubytest -Ilib test/*.rb"
end

desc "render README.rdoc to web/readme.html (need malt)"
task :readme do
  sh "malt README.rdoc > web/readme.html"
end

# if `README.rdoc` changes generate `web/readme.html`.
file 'README.rdoc' do
  sh "malt README.rdoc > web/readme.html"
end

