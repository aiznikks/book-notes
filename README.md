#!/bin/bash

# Specify the output file in the current directory
output_file="./tvn_file_sizes.txt"

# Initialize or clear the output file
> "$output_file"

# Print the header for the table
echo -e "Filename\tSize (bytes)" >> "$output_file"
echo -e "--------\t--------------" >> "$output_file"

# Loop through each folder in the current directory
for dir in */; do
    # Ensure it's a directory
    if [ -d "$dir" ]; then
        # Find any .tvn file in the current directory (excluding .triv24.tvn)
        find "$dir" -maxdepth 1 -type f -name "*.tvn" ! -name ".triv24.tvn" | while read -r tvn_file; do
            # Get the size of the file
            file_size=$(stat -c%s "$tvn_file")
            
            # Append the filename and size to the output file in tabular format
            printf "%-20s\t%d\n" "$(basename "$tvn_file")" "$file_size" >> "$output_file"
        done
    fi
done

echo "Task completed. The names and sizes of .tvn files have been written to $output_file."
