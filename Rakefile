require 'rubygems'
require 'psych'
require 'git'

task :default => [:run]


task :run, :folder do |t, args|
  args.with_defaults(:folder => 'gists')
  Dir.chdir(args[:folder])
  Rake::Task['update_existing_gists'].invoke
  Rake::Task['get_new_gists'].invoke
end

task :get_new_gists do
  gists = gist_list
  gists.each do |name, hash|
    unless File.directory?(name)
      puts "Initial pull of #{name}"
      Git.clone("git@github.com:#{hash}.git", name)
    end
  end
end

task :update_existing_gists do
  gists = gist_list
  Dir.entries('.').select{|f| gist_list.has_key?(f)}.each do |dir|
    begin
      g = Git.open(dir)
      g.remote('origin').fetch

      local_head = g.object('HEAD').sha
      remote_head = g.object('remotes/origin/master').sha

      if local_head == remote_head
        puts "Already up-to-date: #{dir}"
      else
        local_log = g.log.inject([]){|arr,l| arr << l.sha; arr}
        if local_log.include?(remote_head)
          puts "Local ahead of remote: #{dir}"
        else
          g.merge('remotes/origin/master')
          puts "Fast-forward merge: #{dir}"
        end
      end

    rescue ArgumentError
      puts "ERROR: #{dir} is not a git repository"
    rescue
      puts "ERROR: Something is wrong with #{dir}. cd in there and fix it!"
    end
  end
end


def gist_list
  gists = Psych.load File.new('manifest.yaml')

  # convert key to filename
  gists.keys.each do |key|
    tmp = gists[key]
    gists.delete(key)
    gists[key.gsub(/\s/,'-').gsub(/[^a-zA-Z0-9\-_]/, '')] = tmp
  end
  return gists
end
