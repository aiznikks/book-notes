#!/bin/bash

# Check if the directory is provided as an argument
if [ -z "$1" ]; then
  echo "Usage: ./organize_files.sh <directory>"
  exit 1
fi

# Change to the specified directory
cd "$1" || exit

# Loop through all the files in the directory
for file in *; do
  # Check if it's a file (and not a directory)
  if [ -f "$file" ]; then
    # Create a folder name that includes both the base name and the extension
    folder_name="${file%.*}_$(echo ${file##*.})_file"
    
    # Create the folder
    mkdir -p "$folder_name"
    
    # Move the file into the newly created folder
    mv "$file" "$folder_name"
    
    echo "Moved $file to $folder_name/"
  fi
done

echo "All files have been organized into their own folders."
