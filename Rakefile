desc 'deploy to server via rsync'
task :deploy do
  # uploads ALL files b/c I often do site-wide changes and prefer overwriting all
  puts 'deploying'
  sh "rsync -rtzh --progress --delete _site/ --rsh='ssh -pXX' YOUR_SERVER:path/to/blog"
  puts 'done!'
end

desc 'create new post or bit. args: type (post, bit), title, future (# of days)'
# rake new future=0 title="New post title goes here" slug="slug-override-title"
task :new do
  require 'rubygems'
  require 'chronic'
  
  title = ENV["title"] || "New Title"
  future = ENV["future"] || 0
  slug = (ENV["slug"] ? ENV["slug"].gsub(' ','-').downcase : nil) || title.gsub(' ','-').downcase

  if future.to_i < 3
    TARGET_DIR = "_posts"
  else
    TARGET_DIR = "_drafts"
  end

  if future.to_i.zero?
    filename = "#{Time.new.strftime('%Y-%m-%d')}-#{slug}.markdown"
  else
    stamp = Chronic.parse("in #{future} days").strftime('%Y-%m-%d')
    filename = "#{stamp}-#{slug}.markdown"
  end
  path = File.join(TARGET_DIR, filename)
  post = <<-HTML
--- 
layout: post
title: "TITLE"
date: DATE
---

HTML
  post.gsub!('TITLE', title).gsub!('DATE', Time.new.to_s)
  File.open(path, 'w') do |file|
    file.puts post
  end
  puts "new #{type} generated in #{path}"
  system "open -a textmate #{path}"
end
