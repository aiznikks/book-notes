#!/bin/bash

# Loop through all subdirectories at depth level 1 in the current directory
for dir in */; do
    # Check if there's a .tflite file in the current subdirectory
    tflite_file=$(find "$dir" -maxdepth 1 -type f -name "*.tflite" | head -n 1)
    
    # Check if config.cfg exists in the same folder
    config_file="$dir/config.cfg"
    
    if [[ -f "$tflite_file" && -f "$config_file" ]]; then
        # Extract the prefix of the .tflite file (filename without extension)
        prefix=$(basename "$tflite_file" .tflite)
        
        echo "Processing folder: $dir"
        echo "Replacing 'STDnet_v3' with '$prefix' in $config_file"

        # Use sed to replace all occurrences of STDnet_v3 with the prefix
        sed -i "s/STDnet_v3/$prefix/g" "$config_file"
    else
        echo "Skipping folder: $dir (either .tflite or config.cfg missing)"
    fi
done
