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
    # Remove the file extension to create the folder name
    folder_name="${file%%.*}"
    
    # Create the folder (if it doesn't exist)
    mkdir -p "$folder_name"
    
    # Move the file into the corresponding folder
    mv "$file" "$folder_name"
    
    echo "Moved $file to $folder_name/"
  fi
done

echo "All files have been organized into their own folders."
