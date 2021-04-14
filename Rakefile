require 'csv'

STORE = "dl_store.csv"

task :default => :new

desc "Add new set, download, and rename"
task :new do
  puts "Enter URL for thing to download"
  url = STDIN.gets.chomp
  puts "Enter artist"
  artist = STDIN.gets.chomp
  puts "Enter desired title"
  title = STDIN.gets.chomp

  new_title = title.gsub(/[[:space:]]/, '-')
  new_artist = artist.gsub(/[[:space:]]/, '-')
  future_file_name = "#{new_artist}--#{new_title}.opus"
  download(url, future_file_name)
  add_to_log(url, future_file_name)

  sh 'git add .'
  sh "git commit -m 'Set: #{new_artist} -- #{new_title}'"
  sh 'git push'
end

desc "Pick random set and play"
task :play do
  file = Dir.glob("*.opus").sample
  sh "open '#{file}'"
end

desc "Download the parts of the store not local yet"
task :dl do
  current_dir = Dir.entries('.')
  CSV.foreach(STORE) do |row|
    unless current_dir.include?(row[1])
      puts "Downloading '#{row[0]}' to #{row[1]}"
      download(row[0], row[1])
    end
  end
end

def add_to_log(yt_url, file_name)
  CSV.open(STORE, 'a') do |csv|
    csv << [yt_url, file_name]
  end
end

# Downloads a song and stores it as `future_file_name`
def download(yt_url, future_file_name)
  dir = Dir.entries('.')
  sh "youtube-dl -qx #{yt_url}"
  new_dir = Dir.entries(".")
  created_song =  new_dir - dir
  mv(created_song[0], future_file_name, verbose: false)
end
