#!/bin/bash

# Define the target directory to store copied log files
target_dir="./copied_logs"

# Create the target directory if it doesn't exist
mkdir -p "$target_dir"

# Loop through each folder in the current directory
for dir in */; do
    # Ensure it's a directory
    if [ -d "$dir" ]; then
        # Define the path to the log.txt file
        log_file="${dir}log.txt"

        # Check if the log.txt file exists in the directory
        if [ -f "$log_file" ]; then
            # Extract the folder name without trailing slash
            folder_name=$(basename "$dir")
            
            # Construct the new file name with folder name as prefix
            new_log_file="${target_dir}/${folder_name}_log.txt"
            
            # Copy the log.txt file to the target directory and rename it
            cp "$log_file" "$new_log_file"
            
            echo "Copied $log_file to $new_log_file"
        else
            echo "No log.txt found in $dir"
        fi
    fi
done

echo "All log.txt files have been copied and renamed."
