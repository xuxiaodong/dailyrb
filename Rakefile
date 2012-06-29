# Rakefile based off the one in Octopress

require "highline/import"
require "stringex"

## -- Misc Configs -- ##
source_dir      = "."      # source file directory
posts_dir       = "_posts" # directory for blog files
new_post_ext    = "md"     # default new post file extension when using the new_post task

#######################
# Working with Jekyll #
#######################

# usage rake new_post[my-new-post] or rake new_post['my new post'] or rake new_post (defaults to "new-post")
desc "Begin a new post in #{source_dir}/#{posts_dir}"
task :new_post, :title do |t, args|
  raise "### You haven't set anything up yet." unless File.directory?(source_dir)
  mkdir_p "#{source_dir}/#{posts_dir}"
  args.with_defaults(:title => 'new-post')
  title = args.title
  filename = "#{source_dir}/#{posts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title.to_url}.#{new_post_ext}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/&/,'&amp;')}\""
    post.puts "category: "
    post.puts "---"
    post.puts ""
    post.puts ""
  end
  #system "vim #{filename}"
end
