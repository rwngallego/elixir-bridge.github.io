require "rubygems"
require "tmpdir"

require "bundler/setup"
require "jekyll"


# Change your GitHub reponame
GITHUB_REPONAME = "rwngallego/elixir-bridge.github.io"

namespace :site do
  desc "Generate blog files"
  task :generate do
    Jekyll::Site.new(Jekyll.configuration({
      "source"      => ".",
      "destination" => "_site"
    })).process
  end


  desc "Generate and publish blog to gh-pages"
  task :publish => [:generate] do
    Dir.mktmpdir do |tmp|
      cp_r "_site/.", tmp

      pwd = Dir.pwd
      Dir.chdir tmp

      system "git init"
      system "git add ."
      message = "Site updated at #{Time.now.utc}"
      system "git commit -m #{message.inspect}"
      system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
      system "git push origin master:refs/heads/gh-pages --force"

      Dir.chdir pwd
    end
  end
end

desc "Create a new post with the command 'rake new_post['post title here']'"
task :new_post, [:post_name, :image_dir] do |t, args|
  time = Time.now
  date = time.strftime('%Y-%m-%d')
  slug = args.post_name.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '') #slug the title, to make a filemanmt

  file = File.open("_posts/#{date}-#{slug}.markdown", "w")
  file.puts <<-eom
---
layout: page
title: #{args.post_name}
date: #{time.strftime('%Y-%m-%d %H:%M:%S %z')}
---

eom
  file.close
end
