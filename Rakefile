task :default => :generate

desc 'Create new post'
task :post, [:title] do |t, args|
  if args.title then
    new_post(args.title)
  else
    puts "rake create post-name"
  end
end

desc 'Build site with Jekyll'
task :generate => :clean do
  `jekyll`
end

desc 'Start server'
task :server => :clean do
  `jekyll --server`
end

desc 'Deploy'
task :deploy, [:comment] => :generate do |t, args|
  if args.comment then
    `git commit . -m '#{args.comment}' && git push`
  else
    `git commit . -m 'new deployment' && git push`
  end
end

desc 'Clean up'
task :clean do
  `rm -rf _site`
end

def new_post(title)
  time = Time.now
  filename = "_posts/" + time.strftime("%Y-%m-%d-") + title + '.markdown'
  if File.exists? filename then
    puts "Post already exists: #{filename}"
    return
  end
  uuid = `uuidgen | tr "[:upper:]" "[:lower:]" | tr -d "\n"`
  File.open(filename, "wb") do |f|
    f << <<-EOS
---
title: #{title}
layout: post
guid: urn:uuid:#{uuid}
tags:
  - 
---


EOS
  end
  puts "created #{filename}"
  `git add #{filename}`
end
