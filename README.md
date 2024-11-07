#!/bin/bash

# Loop through all directories in the current directory
for dir in */; do
    # Check if the directory contains any .tvn files
    tvn_file=$(find "$dir" -maxdepth 1 -name "*.tvn" | head -n 1)

    if [[ -n "$tvn_file" ]]; then
        echo "Found .tvn file: $tvn_file"

        # Check if the directory contains any .cfg files
        cfg_file=$(find "$dir" -maxdepth 1 -name "*.cfg" | head -n 1)
        
        if [[ -n "$cfg_file" ]]; then
            echo "Found .cfg file: $cfg_file"

            # Define the part to be added with the .tvn file name
            new_content="
[one-infer]
driver=triv24-ssinfer
command=--loadable $(basename "$tvn_file") --input-spec any --dump-input-buffer-as-tv2b-with-prefix input --dump-output-as-tv2b output
"

            # Append the new content to the .cfg file
            echo "$new_content" >> "$cfg_file"
            echo "Updated $cfg_file with the new content."
        else
            echo "No .cfg file found in $dir."
        fi
    else
        echo "No .tvn file found in $dir."
    fi
done
