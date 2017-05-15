require('rubygems')
require('json')
require('yaml')
require('open-uri')

desc "download babel-browser"
task :fetch_remotes do
  IO.copy_stream(
    open('https://unpkg.com/babel-standalone@6.15.0/babel.min.js'),
    'js/babel.min.js'
  )
end

desc "generate js from jsx"
task :js do
  system "../node_modules/.bin/babel _js --out-dir=js"
end

desc "watch js"
task :watch do
  Process.spawn "../node_modules/.bin/babel _js --out-dir=js --watch"
  Process.waitall
end

task :default => [:js]
