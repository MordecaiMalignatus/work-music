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
  git_push('Set: #{new_artist} -- #{new_title}')
end

desc "Delete set from storage"
task :delete do
  candidates = Dir.glob('*.opus')
  puts "Choose set to delete:"
  candidates.each_with_index do |set,index|
    puts "#{index} -- #{set}"
  end
  number = STDIN.gets.chomp.to_i
  deletion_candidate = candidates[number]
  puts "Deleting #{number} -- #{deletion_candidate}"
  sh "rm '#{deletion_candidate}'", verbose: false

  rows = CSV.read(STORE)
  filtered = rows.filter{|(_url, filename)| filename != deletion_candidate}
  File.write(STORE, filtered.map(&:to_csv).join)

  git_push('Deleted: #{new_artist} -- #{new_title}')
end

desc "Pick random set and play"
task :play do
  file = Dir.glob("*.{opus,m4a}").sample
  sh "open -g -a VLC '#{file}'"
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
  sh "youtube-dl --audio-format opus -qx #{yt_url} -o '#{future_file_name}'", verbose: false
end

def git_push(message)
  sh 'git add .', verbose: false
  sh "git commit -m '#{message}'"
  sh 'git push'
end
